# Sentry Docker 搭建
## 环境
* CentOS 7.5 64位
* docker: 18.09.6
* docker-compose: 1.24.0
* sentry:  [sentry 9.1-onbuild](https://hub.docker.com/_/sentry)

1. 安装 `docker` 和 `docker-compose` 参考: [[Docker]]
2. 如果没有启动 docker。 执行 `systemctl start docker` 启动 docker。 **可执行 `systemctl enable docker` 将启动 docker 加入开机自启**
3. 执行 `sudo yum install git` 安装 git
4. 执行 `git clone https://github.com/getsentry/onpremise.git` 克隆到主机
5. 执行 `cd onpremise` 进入 onpremise 文件夹
6. 执行 `docker volume create --name=sentry-data && docker volume create --name=sentry-postgres` - 使用 `docker volume` 必须手动创建本地数据库和 sentry 卷
7. 执行 `cp -n .env.example .env` - 创建 `.env` 文件
8. 执行 `docker-compose build` - 构建并标记 `docker` 服务。
9. 执行 `docker-compose run --rm web config generate-secret-key` - 生成密钥。将它添加到 `.env` 作为 `SENTRY_SECRET_KEY`的值，还要将其添加到 `docker-compose.yml`中。

.env
![](Sentry%20Docker%20%E6%90%AD%E5%BB%BA/53396589-B29A-40E8-AF11-637EED047BA8.png)

docker-compose.yml:
![](Sentry%20Docker%20%E6%90%AD%E5%BB%BA/426669CC-D42F-45E3-863B-7C93963532B2.png)

11. 执行 `docker-compose run --rm web upgrade` - 构建数据库。 官方文档()说这里会 ~~使用交互式提示创建用户帐户~~ 。在实际操作中却没有提示。::如果没有提示需要执行第 12 步::。
12. 执行 `docker exec -it onpremise_postgres_1 bash` 进入docker容器 执行 **postgres bash** 命令查看是否有数据。

	![](Sentry%20Docker%20%E6%90%AD%E5%BB%BA/0FBDC146-065F-4CA7-B937-7879659BA244.png)

	1. 执行 `psql -h 127.0.0.1 -d postgres -U postgres` 进入postgres数据库
	2. 执行 `select * from sentry_project;` 查看 sentry_project 表是否有数据。
	3. 执行 `select * from sentry_organization;` 查看 sentry_organization 表是否有数据。
	4. 执行 `ctrl + d` 退出shell。

16. 如果没有数据需要添加， 执行 `docker-compose run --rm web shell` 进入sentry的web的shell里面。初始化数据

	1. 执行 `from sentry.models import Project`
	2. 执行 `from sentry.receivers.core import create_default_projects`
	3. 执行 `create_default_projects([Project])`
	4. 执行 `ctrl + d` 退出。

	![](Sentry%20Docker%20%E6%90%AD%E5%BB%BA/FE84042A-3B84-4E53-9FDA-9BC719BAE93A.png)

20. 执行 `docker-compose run --rm web createuser` 创建用户。
	![](Sentry%20Docker%20%E6%90%AD%E5%BB%BA/CD96B0FB-7E8A-472B-B543-B01D6C522477.png)

21. 执行 `docker-compose up -d` - 构建启动容器
22. 在浏览器中输入 `[ip]:9000` 

### 问题

* 在执行 `docker-compose run --rm web upgrade` 的时候。可能会出现没有执行完就退出了终端。

	* 没有执行完就退出了
	![](Sentry%20Docker%20%E6%90%AD%E5%BB%BA/E8E9EDE8-09FA-4223-97D4-D745D48DEB04.png)

	* 正常执行完成
	![](Sentry%20Docker%20%E6%90%AD%E5%BB%BA/C0EF8254-C4E7-4D5D-BC42-03E8D889ED5F.png)

* 登录到项目后点击 Create a sample event 测试时， 会发现是失败的，而且这个时候在项目中产生的错误不会在这个 Issues 列表中展示。

	![](Sentry%20Docker%20%E6%90%AD%E5%BB%BA/16F1D2B1-D1EC-46EA-BDFE-69B813E6682C.png)

#### 排查问题步骤:

执行 `docker container logs <web容器id>` 查看日志。发现在执行 SQL 的时候 没有找到给定名称和参数类型匹配的函数。
![](Sentry%20Docker%20%E6%90%AD%E5%BB%BA/23B4330D-A43B-4CE7-9FED-9C948293F2FD.png)

 解决方案: [Waiting for events… Our error robot is waiting to devour receive your first event - #sentry](https://forum.sentry.io/t/waiting-for-events-our-error-robot-is-waiting-to-devour-receive-your-first-event/4355)

1. 执行 `docker exec -it onpremise_postgres_1 bash` 进入docker容器 执行 **postgres bash** 命令查看是否有数据。
2. 执行 `psql -h 127.0.0.1 -d postgres -U postgres` 进入postgres数据库
3. 执行下面SQL 语句后。在浏览器中点击 Create a sample event 就好了，也正常记录Issue了。

```shell
create or replace function sentry_increment_project_counter( project bigint, delta int) 
returns int as $$ declare new_val int; 
begin loop update sentry_projectcounter set value = value + delta where project_id = project returning value into new_val; 
if found then return new_val; 
end if; 
begin insert into sentry_projectcounter(project_id, value) values (project, delta) returning value into new_val; 	 return new_val; 
exception when unique_violation then end; end loop; 
end $$ language plpgsql;
```
