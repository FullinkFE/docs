# 不同font-size的Text设置alignItems:'baseline'安卓上无效
## 问题描述
View中多个不同font-size的Text设置alignItems:'baseline'，iOS上显示正常，安卓上baseline无效，代码如下
```
<View style={{flexDirection:'row',alignItems:'baseline'}}>
    <Text style={{fontSize:14}}>¥ </Text>
    <Text style={{fontSize:40}}>3000.00</Text>
</View>
```
## 解决方法
用Text组件包裹多个Text组件，形成嵌套Text组件，如下
```
<Text>
    <Text style={{fontSize:14}}>¥ </Text>
    <Text style={{fontSize:40}}>3000.00</Text>
</Text>
```