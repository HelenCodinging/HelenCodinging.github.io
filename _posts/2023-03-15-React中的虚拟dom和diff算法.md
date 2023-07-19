---
title:"React中的虚拟dom和diff算法"
tags:TeXt
---

# JSX的本质
React中不强制使用JSX，它只是一个React的语法糖。它最后会通过babel转化为React.createElement()来创建元素。在我们不使用JSX的时候，我们是通过React.createElement来写时，会出现很繁琐的代码，例如：

```javascript
const divElem=React.createElement('div',{className:'mydiv1'},
	React.createElement('span',{className:"myspan1"},"helloword"));
```
如果时使用JSX,则是转化为：

```javascript
const divElem=<div className="mydiv1">
	<span className="myspan1">Helloword</span>
</div>
```
JSX的代码将更加简洁，更加符合HTML书写的规范。一般的项目中，嵌套的标签层级是非常多的。所以，我们如果使用React.createElement()则会写出很冗长的代码。
JSX语法转化过程:JSX->React.createElement()->虚拟DOM->真实DOM->HTML
JSX的本质是虚拟dom,所以我们在写JSX时写的都是虚拟dom,着也符合React中的设计理念。
# 虚拟DOM的由来
由于我们原生的dom中会集成有大量的属性，有许多的属性是我们不常使用的，当我们操作dom的时候携带着有许多属性的dom，会影响我们的性能。所以引入虚拟dom，虚拟dom中减少了很多不常用的属性。
虚拟dom本质上是一个Object类型的对象，它也会转化为真实的dom。当我们每次调用render()方法的时候，其实我们都会产生一个虚拟dom树。
# Diff算法说明
当我们的数据改变，React会自动调用我们的render()方法，render()方法调用过后我们就会产生一棵新的虚拟dom树。新的虚拟dom树会和旧的虚拟dom树进行比较，当发现有了新的改变之后，就只会更新改变的DOM节点。注意更新DOM的最小粒度为dom标签。假如我们更新了div标签的内容，它会将整个div标签进行重新渲染并且更新到页面中。
在一个列表中，假设在列表的最开添加一个标签，如下:

```javascript
let list=<ul>
	<li>1</li>
	<li>2</li>
	<li>3</li>
/ul>//旧的列表

 list=<ul>
 	<li>0</li>
 	<li>1</li>
 	<li>2</li>
 	<li>3</li>
 </ul> //新的列表
```
当我们一一进行比较的时候，我们可能要进行四次的diff操作的更新。但其实我们有三次是之前的列表就有的，没有必要全都更新。这时，我们就引入key。
# key的内部原理是什么？key为什么最好不要使用index?
(1)简单来书，key是虚拟dom的标识，在更新中起着中要的作用。如果一个标签含有key，那么Diff算法会以key相同的标签来做比较。
（2）详细来说，当状态中的数据发生改变时，React会根据新数据产生新的虚拟dom,比较的规则如下：
I、旧虚拟dom中找到了与新虚拟DOM相同的key:
	a、虚拟DOM中内容没变，直接使用之前的虚拟DOM.
	b、虚拟dom中的内容发生了改变，则生成新的真实DOM,随后替换页面之前的真实DOM.
II、旧虚拟DOM中找到与新虚拟DOM相同的key:
根据数据创建新的DOM,随后渲染到页面中。
## 使用index作为key会引发的问题

```javascript
const list=<ul>
	<li key="0">2012</li>
	<li key="1">2013></li>
	<li key="2">2014</li>
</ul> //旧的list列表

const list=<ul>
	<li key="0">2011</li>
	<li key="1">2012</li>
	<li key="3">2013</li>
	<li key="4">2014</li>
</ul> //新的list
```
如上图：如果使用index作为key的话，这样，如果是逆序插入，由于key的改变，列表所有的标签都需要重新渲染。所以，key一般都需要选用唯一值。