# 状态保持

当触发模块更新的时候，模块的状态会丢失，看下面的页面

``` html
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<title>unload example</title>
<script src='hotload.js'></script>
</head>
<body>
<button id='js-btn'>click me</button>
<script>
   //定义 counter
   define('counter', function () {
      var count = 0;
      var btn = document.getElementById('js-btn');
          
	  function clickHandler(){
		    alert(count);
	  }
      btn.addEventListener('click', clickHandler);
	  this.unload=function(){
		 btn.removeEventListener('click',clickHandler);
	  }
  });

</script>
</body>
</html>
```

点击btn 3次，alert 3，这时再重新定义 counter，就是在浏览器console中再次执行counter的定义

这时点击btn,alert 1，而不是预期的4。

>为什么会这样？因为重新定义counter后，factory被更新，count也变成了0

为了让状态保持，需要用到`this.load,this.unload`

``` js
 //定义counter
  define('counter', function () {
      var count = 0;
      var btn = document.getElementById('js-btn');
	  function clickHandler(){
		    alert(count++);
	  }
      btn.addEventListener('click', clickHandler);
	  this.load=function(presevedData){
		//恢复count的值
		count=preservedData.count;
	  };
	  this.unload=function(){
		 //清理事件
		 btn.removeEventListener('click',clickHandler);
		 //返回的count会被保存起来
		 return count;
	  }
  });
 
```

把状态保存后，无论counter更新多少次，count总能保持它的状态

另一种写法

``` js
 //定义counter
  define('counter', function () {
      var count = 0;
      var btn = document.getElementById('js-btn');
	  var that=this
	  if(!that.isHot){
         btn.addEventListener('click', function clickHandler(){
		    alert(count++);
            //保存 count
            that.data.count=count
	     });
      }
      else{
		 //恢复count
         count=that.data.count
      }
	  
  });
 
```
