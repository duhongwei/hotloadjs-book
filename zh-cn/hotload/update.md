# 模块更新

每个模块都有唯一的id，当出现相同Id的模块时，会触发模块更新，同时更新所有下线模块

为了有直观的认识，建议您亲自实践下。环境准备很简单。首先下载[hotload.js](https://github.com/duhongwei/hotloadjs),在同一目录下，打开记事本，贴下面的html，保存为test.html。

``` html
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<title>hotloadjs upload example</title>
<script src='hotload.js'></script>
</head>
<body>
</body>
</html>
```

用chrome（谷歌）浏览器打开，按F12，弹出console。在console中一步一步贴上代码，执行，就会看到相应输出。


为了方便描述，示例中模块的 id 都手工指定。
``` js
//有一个 苹果，它是“红”的
define('apple',function(){
	var apple='red apple',

	function getApple(){
		return apple;
	}
	return {
		getApple:getApple
	};
});
// red apple
console.log(require('apple').getApple()); 
``` 

苹果模块可以重新被定义，这时会触发更新，新的模块覆盖旧的模块。

``` js
//有一个苹果，它是“绿”的
define('apple',function(){
	var apple='green apple',

	function getApple(){
		return apple;
	}
	return {
		getApple:getApple
	};
});

// green apple
console.log(require('apple').getApple());
```

定义一个直接下线模块 plate，一个间接下线模块 man

``` js
//直接下线plate,把苹果放到plate里	
define('plate',['apple'],function(apple){
	var plate=[];
	plate.push(apple);
	function getPlate(){
		return plate;
	}
	return {
		getPlate:getPlate
	}
});
//间接下线man,man take this plate
define('man',['plate'],function(plate){
	var man={
		take:function(){
			return plate.getPlate();
		}
	};
	return man;
});
//green apple,green apple
console.log(require('plate').getPlate(),require('man').take());
```

更新上线apple为 yellow apple,这会触发所有下线的更新

``` js
define('apple',function(){
	var apple='yellow apple',

	function getApple(){
		return apple;
	}
	return {
		getApple:getApple
	};
});
//yellow apple,yellow apple
console.log(require('plate').getPlate(),require('man').take());
```


