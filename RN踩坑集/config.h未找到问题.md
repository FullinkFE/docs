# config.h file not found in Xcode

### 解决方法

```js
follow below steps 
a.) close your Xcode 
b.) Open terminal with the project (Or you can directly left click your project and drag your folder to closed terminal, [It will automatically take the path from your that corresponding folder]) 
c.) write command 
cd node_modules/react-native/third-party/glog-0.3.4/
d.) Run the configure scripted file by the command
./configure
e.) now close terminal and go to terminal with your project root path. now try final run your iOS project by
react-native run-ios
```

