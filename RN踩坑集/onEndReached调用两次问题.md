# onEndReached 调用两次问题

### 问题描述

在使用FlatList做上拉加载数据的时候，发现onEndReached总是会同时调用两次，据说这个FlatList一个bug。
### 解决办法

```js
onEndReached={()=>{
    setTimeout(()=>{
        if (this.canLoadMore) {//fix 滚动时两次调用onEndReached https://github.com/facebook/react-native/issues/14015
            this.loadData(true)
            this.canLoadMore=false
        }
    },100)
}}
onMomentumScrollBegin={()=>{
    this.canLoadMore=true //fix 初始化时页调用onEndReached的问题
}}
```

