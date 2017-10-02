# 第三方模块

hotloadjs对符合AMD规范的模块是兼容的。所以需要考虑的是不符合AMD规范的模块。

即使不符合AMD规范，但也要符合一定要求才行。

1. 模块需要外界知道的API暴露在外面，内部实现隐藏在内部。

比如下面的模块,保存在文件apple-other.js中

``` js
(function(window){
var apple='red apple';

function getApple(){
 return apple;
}

//对外的接口
window.getApple=getApple

})(this)
```
增加一个文件 apple.js

``` js
define(function(){
	return window.getApple;
});

```

对于不符合AMD规范的第三方文件，需要保存为两个文件，一个是第三方模块other，一个是fix作用的文件。加载顺序 other在fix之前就行了。
