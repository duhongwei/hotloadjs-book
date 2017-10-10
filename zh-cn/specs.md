# 规范

参照了 [AMD](https://github.com/amdjs/amdjs-api/blob/master/AMD.md) 规范，为了实现热替换，有所不同。

dfine,require定义的代码是模块。表面看来，define是定义模块，require是引用模块，实际上legojs把define,require都看成是更新（热替换）的最小代码单元，从更新代码的角度来看，没有区别；从使用的角度看，区别就是define的模块可以通过require得到，require的模块只能执行，不能被引用。

define ,require为全局定义。

模块有两个状态，ready,waiting。

一个模块必须要有一个全局唯一的id。

开发环境中，一个文件只包含一个define或一个require

## define
``` js
define(id?, dependencies?, factory)	
```
 
### id
 id是模块的key，是一个字符串，用来标识模块，这个参数是可选的，如果没有指定id，id根据脚本路径自动生成。

### 依赖

第二个参数，dependencies，定义中模块所依赖模块的数组。依赖模块必须优先级执行，并且执行的结果应该按照依赖数组中的位置顺序以参数的形式传入工厂方法（factory）中。依赖是可选的，如果没有依赖，factory立即执行。

### 工厂方法

第三个参数，factory，为模块初始化要执行的函数。只能为函数，只能被执行一次，必须有返回值。
factory有两个可选的方法，[load](),[unload]()。 load用来还原保存过的数据，unload方法用来做清理工作并把需要保存的值返回。

``` js
factory(){
	...
	this.load=function(presevedData){
		//还原保存过的数据
	}
	this.unload=function(){
		//做一些清理工作，
		...

		//将需要保存的数据返回
		return{
	 	  presevedData
		}
	}
}
```
## require

### 定义require模块

	require(id?,dependencies?, factory)

dependencies, factory和define的解释相同，不再重复。唯一不同的是，factory没有返回值。

### 获取已经ready的模块
``` js
require(id);
```

## 举例
``` js
//定义了一个模块apple,key是 apple
define('apple',function(){
	var apple='red apple',

	function getApple(){
		return apple;
	}
	return {
		getApple:getApple
	};
});

console.log(require('apple').getApple()); // red apple
```
根据规范，require也应该是有一个id的。

``` js
//把苹果卖掉
require(['apple'],function(apple){
//do sell apple	
}
```

在实际应用中，一般define模块是不写id的。id是根据路径自动生成，上线时再把id打包到生成环境中。 可以在后面的[演练]()中体会这一点。

## hotload
hotload也可以叫做更新。
如果出现相同 id 的模块，就会触发hotload。

定义两个名词

> 上线(dependence)
``` js
//我的上线是 a,b
require(['a','b'],function(a,b){});
```
> 下线 (quoter)
``` js
//我的下线是下面两个模块
define('m',function(){});
//m 的直接下线
define('n',['m'],function(){});
//m 的间接下线,n 的直接下线
require(['n'],function(){});'
```

一个模块更新后，它的所有直接下线，间接下线都需要更新。