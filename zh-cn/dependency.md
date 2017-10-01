# 依赖

依赖分为我依赖的和依赖于我的，为了区分并方便描述这两种依赖，定义两个名词

## 名词解释

### 上线(dependence)

	//我的上线是 a,b
	require(['a','b'],function(a,b){});

### 下线 (quoter)
	
	//我的下线是下面两个模块
	define('m',function(){});
	//m 的直接下线
	define('n',['m'],function(){});
	//m 的间接下线,n 的直接下线
	require(['n'],function(){});'

## 执行顺序

只有上线都ready,下线才开始执行factory,否则处于waiting状态。

## 更新（hotload)

一个模块更新后，它的所有直接下线，间接下线都需要更新




