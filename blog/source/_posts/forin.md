---
title: for...in  for...of  forEach()的区别
tags: js
categories: js
type: categories
date: 2019-02-15 01:02:11
---


## for...of 语句

for...of 语句在[可迭代对象](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Iteration_protocols)(包括Array, Map, Set, String, TypedArray, arguments 对象等)上创建一个迭代循环, 调用自定义迭代钩子, 并为每个不同属性( each distinct property )的值的执行语句

例:

``` bash
let iterable = [10, 20, 30];
for (let val of iterable) {
    if (val < 30) {
        val += 1;
    } else {
        val += 2;
    }
    console.log(val);
}
console.log(iterable);
// 11
// 21
// 32
// [10, 20, 30]

迭代 string
let iterable = "boo";

for (let value of iterable) {
  console.log(value);
}
// "b"
// "o"
// "o"
```
可看出,为每个属性的值 的执行语句, 但不修改原数据。 

## for...in 语句

for...in语句以任意顺序遍历一个对象的[可枚举属性]()。对于每个不同的属性，语句都会被执行。

```base
var obj = {a:1, b:2, c:3};
    
for (var prop in obj) {
  console.log("obj." + prop + " = " + obj[prop]);
}

// Output:
// "obj.a = 1"
// "obj.b = 2"
// "obj.c = 3"
```

### 描述
for...in 循环只遍历[可枚举属性](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Enumerability_and_ownership_of_properties)。像 Array和 Object使用内置构造函数所创建的对象都会继承自Object.prototype和String.prototype的不可枚举属性，例如 String 的 indexOf()  方法或 Object的toString()方法。循环将遍历对象本身的所有可枚举属性，以及对象从其构造函数原型中继承的属性（更接近原型链中对象的属性覆盖原型属性）。

更多关于[for...in](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/for...in)。

## forEach ( Array.prototype.forEach() ) 方法 

forEach() 方法对数组的每个元素执行一次提供的函数。

```base
var arr = ['a', 'b', 'c'];

arr.forEach(function(currentValue) {
  console.log(currentValue);
});

// "a"
// "b"
// "c"
```
forEach 方法按升序为数组中含有效值的每一项执行一次callback 函数，那些已删除或者未初始化的项将被跳过（例如在稀疏数组上）。

### 注意:

forEach callback中的 return不能结束forEach循环只能跳过本次循环，然后继续执行下次循环直到结束。

```base
var arr = [1,2,3,4];
arr.forEach((cur)=>{
    if(cur === 3){
        console.log('数字为3');
        return;
    }
    console.log('number',cur);
})
// number 1
// number 2
// 数字为3
// number 4
```

## 区别在于

 for...in 操作一个对象的**可枚举属性**。

 for...of 可迭代对象上创建迭代循环并为不同**属性的值**的执行语句。
 
 forEach() 升序为**数组**每个元素执行一次提供的函数。
 
 三者皆为遍历但遍历点不同。