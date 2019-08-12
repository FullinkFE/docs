# WebView 加载H5页面空白问题

* **H5页面使用DOM storage API导致的页面加载问题**

DOM storage 是HTML5提供的一种标准接口，主要将键值对存储在本地，在页面加载完毕后可以通过JavaScript来操作这些数据，但是Android 默认是没有开启这个功能的，则导致H5页面加载失败，错误日志：
```js
[INFO:CONSOLE(186)] "Uncaught TypeError: Cannot read property 'getItem' of null"
[INFO:CONSOLE(1)] "Error:Uncaught TypeError: Cannot read property 'getItem' of null
[INFO:CONSOLE(1)] "Error:Uncaught TypeError: Cannot read property 'activeIndex' of undefined
```
* **解决方式：**

通过WebSettings设置开启DOM Storage功能
```js
settings.setDomStorageEnabled(true);
```

