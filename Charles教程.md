## 下载安装

[去官网下载软件并安装](https://www.charlesproxy.com/)

或找其他一些破解包、破解License Key；下面方法引自https://www.jianshu.com/p/82f63277d50f，未测试是否还有效。

charles可以免费试用30天，强烈支持购买正版软件，现提供一种破解方法，用于学习交流，Up主用的是charles 4.2.7。
1、 打开charles ---> help---> register。
2、输入Registered Name:  [https://zhile.io](https://zhile.io/) 、 License Key: 48891cf209c6d32bf4。
3、打开Proxy ---->点击 maxOS Proxy，即可开始抓浏览器网页请求。

## 手机抓包

首先需要将 Charles 的代理功能打开。在 Charles 的菜单栏上选择 “Proxy”->”Proxy Settings”，填入代理端口 8888，并且勾上 “Enable transparent HTTP proxying” 就完成了在 Charles 上的设置。如下图所示:

![](https://blog.devtang.com/images/charles-proxy-setting.jpg)

确保手机和电脑连接同一wifi，手机上设置代理，代理服务器地址填电脑当前网络的ip，端口填上面设置的端口，默认8888，iphone上设置如下

![](https://blog.devtang.com/images/charles-iphone-setting.jpg)

设置好之后，打开 iPhone 上的任意需要网络通讯的程序，就可以看到 Charles 弹出 iPhone 请求连接的确认菜单（如下图所示），点击 “Allow” 即可完成设置。

![](https://blog.devtang.com/images/charles-proxy-confirm.jpg)



## https证书配置

参考以下链接配置

https://www.cnblogs.com/someonelikeyou/p/7821533.html

**！！！特别注意如果是iOS11以上 手机 要打开信任开关，设置-->通用-->关于本机-->证书信任设置--> 指定证书开关开启**

![](https://images2017.cnblogs.com/blog/494253/201711/494253-20171112144050075-1020508117.jpg)

安卓设置可以参考以下链接

https://blog.csdn.net/xiaocszn/article/details/86081055

## 常用工具

* 网速模拟

  在 Charles 的菜单上，选择 “Proxy”->”Throttle Setting” 项，在之后弹出的对话框中，我们可以勾选上 “Enable Throttling”，并且可以设置 Throttle Preset 的类型。如下图所示：

  ![](https://blog.devtang.com/images/charles-throttle-setting.jpg)

* Breakpoints

  Breakpoints 功能类似我们在 Xcode 中设置的断点一样，当指定的网络请求发生时，Charles 会截获该请求，这个时候，我们可以在 Charles 中临时修改网络请求的返回内容。

  下图是我们临时修改获取用户信息的 API，将用户的昵称进行了更改，修改完成后点击 “Execute” 则可以让网络请求继续进行。

  ![](https://blog.devtang.com/images/charles-breakpoint.png)

* 更多工具参考以下链接

  [详细介绍](https://blog.devtang.com/2015/11/14/charles-introduction/)

  [高级工具](https://www.jianshu.com/p/6683c837c07a)