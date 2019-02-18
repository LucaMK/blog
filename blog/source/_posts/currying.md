---
title: 柯里化(curry)函数
tags: js
categories: js
type: categories
date: 2019-02-14 01:02:11
---



起因为群里小伙伴提出 如何实现类似 add(1)(2)(3) 等与 6 的类似调用方法，马上就有回答:

```base
var add = function(a) {
    return function(b) {
        return function(c) {
            return a+b+c;
        }
    }
}
add(1)(2)(3); // 6
```

easy,完美解决问题。但是要是add(1)(2)(3)(4) 有4次或者更多更少次的调用那这就不适合了。

这可看作是执行函数返回函数自身值:
```base
function add(x) {
    var sum = x;
    var fn = function (y) {
        sum = sum + y;
        return fn;
    }
    fn.toString = function () {
        return sum;
    }
    retrun fn;
}

console.log(add(1)(2)(3)); // 6
console.log(add(1)(2)(3)(4)); // 10
```
首先要一个数记住每次的计算值，所以使用了闭包，在fn中记住了x的值，第一次调用add(),初始化了fn，并将x保存在fn的作用链中，然后返回fn保证了第二次调用的是fn函数，后面的计算都是在调用fn, 因为fn也是返回的自己，保证了第二次之后的调用也是调用fn，而在fn中将传入的参数与保存在作用链中x相加并付给sum，这样就保证了计算；

但是在计算完成后还是返回了fn这个函数，这样就获取不到计算的结果了，我们需要的结果是一个计算的数字那么怎么办呢，首先要知道JavaScript中，打印和相加计算，会分别调用toString或valueOf函数，所以我们重写fn的toString和valueOf方法，返回sum的值；

其实以上问题就是 柯里化(curry)，又称为部分求值.

## 什么是柯里化 (curry)

curry的概念很简单: 只传递给函数一部分参数来调用它，让它返回一个函数去处理剩下的参数。------摘取自js函数编程指南

你可以一次性的调用curry函数，也可以每次只传一个参数多次调用。
```base
var add = function(x) {
  return function(y) {
    return x + y;
  };
};

var increment = add(1);
var addTen = add(10);

increment(2);
// 3

addTen(2);
// 12
```

我们来创建一些 curry 函数享受下（译者注：此处原文是“for our enjoyment”，语出自圣经）。

```base
var curry = require('lodash').curry;

var match = curry(function(what, str) {
  return str.match(what);
});

var replace = curry(function(what, replacement, str) {
  return str.replace(what, replacement);
});

var filter = curry(function(f, ary) {
  return ary.filter(f);
});

var map = curry(function(f, ary) {
  return ary.map(f);
});
```
我在上面的代码中遵循的是一种简单，同时也非常重要的模式。即策略性地把要操作的数据（String， Array）放到最后一个参数里。到使用它们的时候你就明白这样做的原因是什么了。

```base
match(/\s+/g, "hello world");
// [ ' ' ]

match(/\s+/g)("hello world");
// [ ' ' ]

var hasSpaces = match(/\s+/g);
// function(x) { return x.match(/\s+/g) }

hasSpaces("hello world");
// [ ' ' ]

hasSpaces("spaceless");
// null

filter(hasSpaces, ["tori_spelling", "tori amos"]);
// ["tori amos"]

var findSpaces = filter(hasSpaces);
// function(xs) { return xs.filter(function(x) { return x.match(/\s+/g) }) }

findSpaces(["tori_spelling", "tori amos"]);
// ["tori amos"]

var noVowels = replace(/[aeiou]/ig);
// function(replacement, x) { return x.replace(/[aeiou]/ig, replacement) }

var censored = noVowels("*");
// function(x) { return x.replace(/[aeiou]/ig, "*") }

censored("Chocolate Rain");
// 'Ch*c*l*t* R**n'
```
这里表明的是一种“预加载”函数的能力，通过传递一到两个参数调用函数，就能得到一个记住了这些参数的新函数。