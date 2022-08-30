# Mac 下 Homebrew 的快速安装

> 在 mac 系统中，安装 Homebrew，如果不翻墙是不可能的，但是即使翻墙了，安装过程中也会多次失败，多次尝试，才能最终安装成功，在这样的情况下，我们可以尝试使用国内的镜像。给大家推荐一个中国科学技术大学的镜像站点：https://mirrors.ustc.edu.cn/brew.git

##### 安装步骤

1. 获取 install 文件并执行

   ```shell
   cd ~
   curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install >> brew_install
   ```

2. 编辑 brew_install 文件

   注释掉`BREW_REPO = "https://github.com/Homebrew/brew".freeze`和`CORE_TAP_REPO = "https://github.com/Homebrew/homebrewcore".freeze`

   修改为`BREW_REPO = "git://mirrors.ustc.edu.cn/brew.git".freeze`和`CORE_TAP_REPO = `

   `"git://mirrors.ustc.edu.cn/homebrew-core.git".freeze`

3. 执行脚本

   ```bash
   /usr/bin/ruby ~/brew_install
   ```

然后可以看到这几句：

==> Tapping homebrew/core

Cloning into '/usr/local/Homebrew/Library/Taps/homebrew/homebrew-core'...

<font color='red'>fatal: unable to access 'https://github.com/Homebrew/homebrew-core/': LibreSSL SSL_read: SSL_ERROR_SYSCALL, errno 54</font>

<font color='red'>Error: Failure while executing: git clone https://github.com/Homebrew/homebrew-core /usr/local/Homebrew/Library/Taps/homebrew/homebrew-core --depth=1</font>

<font color='red'>Error: Failure while executing: /usr/local/bin/brew tap homebrew/core</font>

liyuanbadeMacBook-Pro:~ liyuanba$ git clone https://github.com/Homebrew/homebrew-core /usr/local/Homebrew/Library/Taps/homebrew/homebrew-core --depth=1

出现这个原因是因为源不通，代码来不下来，解决方法就是更换国内镜像源：

执行下面这句命令，更换为中国科学技术大学的镜像：

```
git clone git://mirrors.ustc.edu.cn/homebrew-core.git/  /usr/local/Homebrew/Library/Taps/homebrew/homebrew-core --depth=1
```

就下载成功了

4. 替换 Homebrew 源

   替换 homebrew 默认源

   ```bash
   cd "$(brew --repo)"
   git remote set-url origin git://mirrors.ustc.edu.cn/brew.git
   ```

   替换 homebrew-core 源

   ```bash
   cd "$(brew --repo)/Library/Taps/homebrew/homebrew-core"
   git remote set-url origin git://mirrors.ustc.edu.cn/homebrew-core.git
   ```

   Brew 更新

   ```bash
   brew update
   ```

   设置 bintray 镜像

   ```bash
   echo 'export HOMEBREW_BOTTLE_DOMAIN=https://mirrors.ustc.edu.cn/homebrew-bottles' >> ~/.bash_profile
   source ~/.bash_profile
   ```

5. 最后用这个命令检查无错误：

   `brew doctor`

   至此 Homebrew 安装成功

> 总结：按照这个安装方法，全程只需要翻墙一次，即获取 brew_install 文件的时候，其余时间均不需要翻墙，而且全程安装较快，推荐的 Homebrew 安装方法
>
> > 参考链接：https://www.jianshu.com/p/6523d3eee50d
> >
> > https://blog.csdn.net/qq_35624642/article/details/79682979

---

> 2022 年了，以上的脚本估计太老了，可以采用以下的方法一键安装

```bash
/bin/zsh -c "$(curl -fsSL https://gitee.com/cunkai/HomebrewCN/raw/master/Homebrew.sh)"
```

> 遇到的奇葩问题：
> 安装 homebrew 和 nvm 的过程中经常遇到 443 的错误

![错误图片](https://user-images.githubusercontent.com/11072796/82433212-b5f11f80-9ac3-11ea-886e-6fe17edc1d2e.png)

以上是安装 pnpm 的报错信息，可以发现，脚本需要到 raw.githubusercontent.com 上拉取代码，导致啦取失败是因为 github 的一些域名的 DNS 解析被污染，导致 DNS 解析过程无法通过域名取得正确的 IP 地址

#### 解决方案

打开 https://www.ipaddress.com/ 输入访问不了的域名
![网址截图](https://user-images.githubusercontent.com/11072796/82434255-2e0c1500-9ac5-11ea-8102-9ebe8475ea34.png)

查询之后可以获得正确的 IP 地址

在本机的 host 文件中添加，建议使用 switchhosts 方便 host 管理

199.232.68.133 raw.githubusercontent.com

> 参考链接：
> https://zhuanlan.zhihu.com/p/111014448
