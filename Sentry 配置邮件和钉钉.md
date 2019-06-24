# Sentry 配置邮件和钉钉
## 邮件配置
1. 在 `docker-compose.yml`中配置邮件。
```yaml
{
 	# 邮箱配置
	SENTRY_EMAIL_HOST: smtp
	SENTRY_EMAIL_HOST: ‘smtp.qq.com’
	SENTRY_EMAIL_USER: ‘604389771@qq.com’
	SENTRY_EMAIL_PASSWORD: ‘xxxxx’   # 邮箱授权码
	SENTRY_SERVER_EMAIL: ‘604389771@qq.com’
	SENTRY_EMAIL_PORT: 587        # 阿里云必须写这一项
	SENTRY_EMAIL_USE_TLS: ‘true’   # 阿里云必须写这一项
}
```

2. 执行完成后 重新构建和重启
```shell
docker-compose build
docker-compose up -d
```

3. 重新登录后找到 用户管理 > 邮件 进行查看，可以点击 发送测试邮件测试。
![](Sentry%20%E9%85%8D%E7%BD%AE%E9%82%AE%E4%BB%B6%E5%92%8C%E9%92%89%E9%92%89/1B018688-34F7-4F7A-B545-4EEAE2096ABA%203.png)

![](Sentry%20%E9%85%8D%E7%BD%AE%E9%82%AE%E4%BB%B6%E5%92%8C%E9%92%89%E9%92%89/2AEBEC99-5467-442F-B1C0-9CABEDD2415E%203.png)；。、

* 钉钉配置，配置钉钉机器人通知需要下载一个 [GitHub - anshengme/sentry-dingding: Sentry钉钉通知](https://github.com/anshengme/sentry-dingding)。[安装插件教程](https://blog.ansheng.me/article/docker-sentry-django-email-dingtalk/)

1. 创建钉钉群组并添加机器人，获取到 `access_token`。
2. 在 Sentry-web 中从插件列表中找到钉钉
![](Sentry%20%E9%85%8D%E7%BD%AE%E9%82%AE%E4%BB%B6%E5%92%8C%E9%92%89%E9%92%89/C58619D8-3273-414F-971D-3A18F55C8666%203.png)
3. 点击 Configure Plugin. 将钉钉中获取到`access_token` 填入。
![](Sentry%20%E9%85%8D%E7%BD%AE%E9%82%AE%E4%BB%B6%E5%92%8C%E9%92%89%E9%92%89/19AC08EE-8879-49CD-94B1-4944C2EC2DB8%203.png)



