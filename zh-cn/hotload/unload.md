# 清理模块


当触发模块更新的时候，需要对旧模块做清理工作，否则会出现问题。比如有页面如下

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
    //定义counter
    define('counter', function () {
        var count = 0;
        var btn = document.getElementById('js-btn');
        
        btn.addEventListener('click', function () {
            alert('cick');
        });
    });
    //重新定义counter
    define('counter', function () {
        var count = 0;
        var btn = document.getElementById('js-btn');
        
        btn.addEventListener('click', function () {
            alert('cick');
        });
    });
</script>
</body>
</html>
```

预期：点击btn一次,alert一次

实际：点击btn一次,alert两次

>为什么会这样？因为重新定义counter的时候，btn的事件又被绑定了一次，导致btn click事件有两个handler。

解决这个问题，需要用到 `this.unload`

``` js
 //有清理功能的counter
  define('counter', function () {
      var count = 0;
      var btn = document.getElementById('js-btn');
	  function clickHandler(){
		    alert('click');
	  }
      btn.addEventListener('click', clickHandler);
	  this.unload=function(){
		 btn.removeEventListener('click',clickHandler);
	  }
  });
 
```

在Unload方法中清理了事件绑定后， 无论counter更新多少次，一切正常了