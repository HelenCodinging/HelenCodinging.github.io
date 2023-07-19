---
title："let、const、var的区别"
tags:"TeXt"
---

# let和const产生的背景
在JS声明变量的时候，我们最初使用的是var进行声明。由于var存在可以重复定义、变量提升（在还没有用var进行变量声明时还能访问变量），与其他的语言声明变量的规则不符合，使得很多初学者会出现bug，为了解决这个问题，引入了let和const。
# let和const的区别
##### 1.var没有块级作用域，let和const有块级作用域。

```javascript
for(var i=0;i<5;i++)
{
	console.log(i)
}
console.log(i) //5
```

```javascript
for(let i=0;i<5;i++)
{
	console.log(i);
}
cosnsole.log(i);//浏览器会报错
```

##### 2.var有变量提升，let和const没有变量提升。在let和const之前使用变量，会触发暂时性死区。

```javascript
console.log(a);// 报错：a is not defined
```

```javascript
console.log(a);// 报错：Cannot access 'a' before initialization
const a=10;
```
##### 3.var声明的变量可以重名，let和const声明的变量不能在同一个作用域中同名。

```javascript
if(1)
{
	var a=1;
	let b=2;
	const c=3;
}
console.log(a); //1
console.log(b);  //报错：b is not defined
console.log(c); //报错：c is not defined.
```
##### 4.const定义的变量不能在后续更改它的值（指的是它的地址值）

```javascript
const p={
 name:"Jack",
 age:10;
 }
 p.name="Amy"
 p={
  name:"Amy"
  age:19
  }//报错：不能更改它的对象引用
```
# 原理
var声明变量的时候，会在栈内存里预分配一个内存空间，等到实际语句执行的时候，再存储一个变量。而let和const是不会在栈内存里预分配内存空间，在栈内存分配变量时，还会做一个检查，是否已经有同名的变量，如果有同名的变量就会报错。