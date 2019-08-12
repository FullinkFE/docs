# react-native WebView 加载html代码一直loading

### 问题描述

 项目中协议预览，接口返回的是html代码，此时webview的属性设置如下，运行时webview一直显示loading中
```js
<WebView  source={{html:'htmlSource' }}/>
```
### 解决方法

如下设置webview 的baseUrl为" "即可
```js
<WebView  source={{html:htmltext, baseUrl: "" }}/>
```

