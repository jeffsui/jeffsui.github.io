title: "javascript oop 15 min programming"
date: 2015-04-23 13:13:30
categories: javascript
tags:

- javascript
- oop

---
#javascript的面向对象15分钟教程

第一种面向对象的写法
--

##创建空对象

```

var bill ={};//创建一个空对象

```

##给对象添加属性和方法

```

bill.name = "Bill Goat";
bill.work = function (){
    console.log("programming....");
};

```
##一步完成上面的两件事

```

var bill ={
    name : "Bill Goat";
    work : function(){
    console.log("programming....");
}
};

```
##访问对象和属性

```

console.log(bill.name);
bill.work();

```

##方法重写

```

bill.name = "Bill Goat";
bill.work =function(who){
     console.log("programming for "+who);
};

```
##通过this关键字访问内部属性

```

bill.say = function (){
    console.log("bill's name  is"+this.name);
};

```

##对象引用
```

var silly = bill;
console.log(silly.name);
sally.name = "Silly";
console.log(silly.name);
console.log(bill.name);

```

##另一个方式引用
```

bill.name = "Bill Goat";
bill.say();

var sayName = bill.say();
sayName;
sayName();

```
##有意思的地方 ：全局属性
```

var name = "Global";
bill.say();

```

发现此时输出的是
`bill's name is Global`

另一种面向对象的写法
--

##定义对象及属性

```

function Game(){};

```

##创建对象

```
var DF = new Game();

```
##对象属性

```
DF.title = "星际争霸2";

```
##构造方法

```

function Game (title){
	this.title = typeof title !== 'undifined' ? title :"";
};

var d3 = new Game("d3");
d3.title;
d3.title ="starcraft2";


```

`this.title = typeof title !== 'undifined' ? title :"";`相当于

```

if(typeof title !== "undifined"）{
	this.title = title;
}else{
	this.title = "";
}

```

##创建一个方法来访问这个属性
```

d3.loveTitle = function (){
	console.log("I love "+this.title);
}

```
##更好的写法

```

Game.prototype.heartIt = function (){
	console.log("I love "+this.title);
}
d3.heartIt();

```

下次详解javascript的原生对象模型

to be continued~~~~