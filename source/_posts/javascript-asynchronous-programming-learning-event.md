title: "javascript asynchronous programming learning :event"
date: 2015-04-20 21:05:16
category: javascript
tags:

- javascript
- 异步


---
#javascript异步编程读书笔记之事件机制

##事件的调度

* 异步执行

`setTimeout`函数的解释:给定一个回调及n毫秒的延迟,setTimeout会在n毫秒后运行该回调。

代码清单1:
```
for (var i = 1; i <= 3; i++) {
	setTimeout(function(){ 
		console.log(i); }, 
	0);
};
```
输出结果:

```
4
4
4
```

* 线程阻塞
代码清单2:
```
var start = new Date;
setTimeout(function(){
var end = new Date;
console.log('Time elapsed:', end - start, 'ms');
}, 500);
while (new Date - start < 1000) {};
```

* 队列

javascript使用队列的方式来循环处理请求,这种机制被称为**事件循环**。