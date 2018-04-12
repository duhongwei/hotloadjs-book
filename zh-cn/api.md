## API参考 

## define

全局定义

``` js
define(id?, dependencies?, factory)	
```

- id
	- 类型：String
	- 模块标识

	如果id没有定义，会自动生成一个，详情请见 [如何生成id](id.md)

- dependencies
	- 类型：Array
	- 模块的上线（依赖模块）

	如果有dependencies为空，模块立即执行factory,进入ready状态。如果不为空，需要所有上线全部ready才会执行factory,进入ready状态

- factory
	- 类型：function
	- 模块初始化要执行的函数，必须有返回值。只能被执行一次，必须有返回值。
	- load方法，发生热替换后恢复模块的数据，详见 [状态保持](hotload/hold.md)

	``` js
		define(function(){
			this.load=function(preservedData){
			  //preservedData是一个在unload方法通过返回值保存下来的数据对象，可以恢复factory中的变量值。
			}
		})
	```
	- unload方法，发生热替换后做模块的清理工作，并用返回值来保存变量值。详见 [清理模块](hotload/unload.md)

	``` js
		define(function(){
			this.unload=function(){
			  //做些清理工作
			  //把要保存的数据返回
			  return {
                //需要保存的值
			  }
			}
		})
	```

## require

同 define。唯一不同的是 require的 factory没有返回值


----------
下面说下内置模块lego的方法。首先需要获得内置模块

``` js
var lego=require('lego');
```

在lego中有如下几个方法

## lego.inspect

``` js
	require('lego').inspect();
```

详见 [调试](debug.md)

## lego.load

``` js
	require('lego').load(filePath);
```
- filePath
	- 类型：String
	- 需要加载的脚本的网址。

## load event
当模块加载时会发出一个事件,可以在监听事件做相应处理

	require(['lego'], function (lego) {
  		lego.on('load', function (mod) {
		})
	})

