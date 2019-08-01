# Android 8.0踩坑记录
## Only fullscreen opaque activities can request orientation
### 原因：
Google在安卓8.0版本时为了支持全面屏，增加了一个限制：如果是透明的Activity，则不能固定它的方向，因为它的方向其实是依赖其父Activity的（因为透明）。然而这个bug只有在8.0中有，8.1中已经修复。
### 解决办法：
  1. 用户手机升级系统版本;
  2. 在代码层判断用户的手机系统是否为Android O也就是Android 8.0，同时全屏属性和透明属性设置，需要禁掉设置方向方法。