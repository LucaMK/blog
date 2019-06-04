---
title: JavaScript 中常用标准对象, 
tags: js js_foundation
---

写本篇的目的在于对JavaScript 标准对象及其方法的回顾。

标准对象有哪些?
	常用的有: Array, Boolean, Date, Error, Function, JSON, Math, Number, Object, RegExp, String, Map, Set, WeakMap , WeakSet, Promise, Proxy...

##Object: Object 构造函数创建一个对象包装器。

Object 构造函数为给定值创建一个对象包装器。如果给定值是null或undefined, 将会创建并返回一个空对象, 否则, 将返回一个与给定值对应类型的对象。

当以非构造函数形式被调用时, Object等同于 new Object().