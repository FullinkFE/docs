# mac 系统（unix）下nginx 安装

> 之前安装nginx，如果在windows下，操作简便，基本解压后打开对应文件夹，点击nginx.exe就能使用，如果在mac下，会使用**软件包的管理器 Homebrew**，但是这次由于没有购买翻墙VPN（PS：不使用VPN的话，安装Homebrew基本不行），决定试一下不安装软件包管理器：yum ,rpm ,Homebrew等的情况下，徒手安装。



nginx安装需要依赖以下几个库：

1. PCRE库
   PCRE库支持正则表达式。如果我们在配置文件nginx.conf中使用了正则表达式，那么在编译Nginx时就必须把PCRE库编译进Nginx，因为Nginx的HTTP模块需要靠它来解析正则表达式。

2. zlib库

   zlib库用于对HTTP包的内容做gzip格式的压缩，如果我们在nginx.conf中配置了gzip on，并指定对于某些类型（content-type）的HTTP响应使用gzip来进行压缩以减少网络传输量，则在编译时就必须把zlib编译进Nginx。

3. OpenSSL库

   如果服务器不只是要支持HTTP，还需要在更安全的SSL协议上传输HTTP，那么需要拥有OpenSSL。另外，如果我们想使用MD5、SHA1等散列函数，那么也需要安装它。

由于Mac 系统自带了Gcc，所以gcc就不需要再安装了。



##### 安装步骤

安装nginx前面必须安装pcre,zlib和ssl

1. 安装pcre

   首先下载pcre，参考地址是：http://nchc.dl.sourceforge.net/project/pcre/pcre2/10.20/pcre2-10.20.tar.bz2

   ```shell
   tar -xvf pcre2-10.20.tar.bz2
   cd pcre2-10.20
   sudo ./configure
   sudo make
   sudo make install
   ```

2. 安装zlib

   下载zlib，参考地址：http://zlib.net/zlib-1.2.8.tar.gz

   ```shell
   tar -xvf zlib-1.2.8.tar.gz
   cd zlib-1.2.8
   sudo ./configure
   sudo make
   sudo make isntall
   ```

3. 安装ssl

   下载openssl, 参考地址：http://www.openssl.org/source/openssl-1.0.1o.tar.gz

   ```shell
   tar openssl-1.0.1o.tar.gz
   cd openssl-1.0.1o
   sudo ./config
   sudo make
   sudo make install
   ```

4. 安装nginx

   下载nginx，参考地址：http://nginx.org/download/nginx-1.2.8.tar.gz

   ```shell
   tar -xvf nginx-1.2.8.tar.gz
   cd nginx-1.2.8
   sudo ./configure --prefix=/usr/local/nginx
   sudo make 
   sudo make install
   ```



当流程走到第4步，安装nginx时，执行命令sudo ./configure --prefix=/usr/local/nginx后，发现报错：$\color{red}{./configure: error: the HTTP rewrite module requires the PCRE library}.$ 可是已经安装过PCRE库了，这是什么原因导致的呢，百度数次后，无果，奈何开始安装Homebrew，安装Homebrew后，执行 `brew install pcre `后，再次执行第4步，搞定

5. 启动nginx

   /usr/local/nginx/sbin/nginx

   打开localhost或者127.0.0.1
   打开浏览器，如果是Welcome to nginx!，说明启动成功



但是在命令行中输入nginx ，此时提示 $\color{red}{-bash: nginx: command not found }$，推测是nginx路径没有配置到环境变量里面，按照如下配置进行：

- 进入`vi /etc/profile`文件

- 添加配置如下：

  ```shell
  PATH=$PATH:/usr/local/nginx/sbin
  export PATH
  ```

  保存并退出

- `source /etc/profile ` 让配置文件重新生效一下

- 命令行中输入sudo nginx  -s stop生效，表示nginx命令存在了



>
>
>综上所述：不要省VPN的小钱，安装个Homebrew吧，Homebrew安装软件包，会自动安装软件包所依赖的库，简单、方便、实用
>
>> 参考链接：https://blog.csdn.net/rightbeforethesix/article/details/93175086
>>
>> https://www.jianshu.com/p/14c81fbcb401
>>
>> https://www.cnblogs.com/zrbfree/p/6419043.html
>>
>> https://blog.csdn.net/chendaoqiu/article/details/46788651