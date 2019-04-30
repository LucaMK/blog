---
title: animation 的使用
tags: css
date: 2019-04-30 14:02:42
---


###animation 示例

![animation示例](/images/animation.gif)

```base
<!-- cube 立方体 -->
<div class="foundation">
	<div class="frount"></div>
	<div class="back"></div>
	<div class="left"></div>
	<div class="right"></div>
	<div class="top"></div>
	<div class="bottom"></div>
</div>
//.css

.foundation{
	width: 100px;
	height: 100px;
	margin-left: 50px;
	position: relative;
	transform-style:  preserve-3d ;
	transform:perspective(300px) translate3d(10px, 10px, 10px) ;
	animation: 5s ease-in-out infinite circle alternate;
}
.foundation>div{
	width: 100px;
	height: 100px;
	background-color: rgba(0,0,0, .1);
	position: absolute;
	box-sizing: border-box;
	border: 1px solid grey;
}

.frount{
	transform-origin: center;
	transform: translate3d(0, 0, 50px);
}
.back{
	transform: translate3d(0, 0, -50px);
}
.left{
	transform:  rotateY(90deg);
	left: -50px;
}
.right{
	transform-origin: center;
	transform: translate3d(50px, 0, 0) rotateY(90deg);
}

.top{
	transform-origin: center;
	transform:translate3d(0, -50px, 0) rotateX(90deg);
}

.bottom{
	transform-origin: center;
	transform: translate3d(0, 50px, 0) rotateX(90deg);
}

@keyframes circle{
	from {transform: rotateX(-10deg) rotateY(0deg) rotateZ(0deg) scale3d(0, 0, 0);}
	to  {transform: rotateX(-10deg) rotateY(360deg) rotateZ(0deg) scale3d(1, 1, 1);}
}
```

## 为什么使用animation , 当让是将原本的静态页面动起来, 增强体验度。

使得可以将从一个CSS样式配置转换到另一个CSS样式配置。动画包括两个部分:描述动画的样式规则和用于指定动画开始、结束以及中间点样式的[关键帧@keyframes](https://developer.mozilla.org/zh-CN/docs/Web/CSS/@keyframes)。

相对于传统脚本实现动画技术, 使用css动画(animation)有三个主要有点:

1. 能够非常容易的创建简单动画, 你甚至不需要了解JavaScript就能创建动画。

2. 动画运行效果良好, 甚至在低性能的系统上。喧染引擎会使用跳帧或者其他技术以保证动画表现尽可能流畅。而使用JavaScript实现的动画通常表现不佳(除非经过很好的设计)。

3. 让浏览器控制动画序列, 允许浏览器优化性能和效果, 如降低位于隐藏选项卡中的动画更新频率。

##配置animation 动画

创建动画序列，需要使用animation属性 或其子属性，该属性允许配置动画时间、时长以及其他动画细节，但该属性不能配置动画的实际表现，动画的实际表现由@keyframes规则实现

###关于关键帧 @keyframes

一但完成动画的时间设置，接下来就需要定义动画的表现。 使用关键帧 @keyframes 创建两个以上的关键帧来实现。每一个关键帧都描述了动画元素在给定时间上该如何喧染。关键帧使用percentage来指定动画发生时间点。0%/from 第一时刻，100%/to 最终时刻。

```base

@keyframes slidein {
  from {
    margin-left: 100%;
    width: 300%;
  }

  to {
    margin-left: 0%;
    width: 100%;
  }
}
```

##animation的子属性

1. animation-delay : 设置延迟时间。(注意: 设置时间必须带上单位 s / ms, 不然则无效)

2. animation-direction : 设置动画在每次运行完后是反向运行还是重新回到开始位置重复运行。
	direction可选值有:
	1. normal
		每个循环内动画向前循环，换言之，每个动画循环结束，动画重置到起点重新开始，这是默认属性。
	2. alternate
		动画交替反向运行，反向运行时，动画按步后退，同时，带时间功能的函数也反向，比如，ease-in 在反向时成为ease-out。计数取决于开始时是奇数迭代还是偶数迭代
	3. reverse
		反向运行动画，每周期结束动画由尾到头运行。
	4. alternate-reverse
		反向交替， 反向开始交替
		动画第一次运行时是反向的，然后下一次是正向，后面依次循环。决定奇数次或偶数次的计数从1开始。
3. animation-duration : 设置动画周期时长。(注意: 设置时间必须带上单位 s / ms, 不然则无效)

4. animation-iteration-count : 设置动画重复次数。
	可选值:
	1. infinite 无限
		无限循环播放动画.
	3. number 数值
		动画播放的次数 不可为负值. 可以用小数定义循环(0.5 将播放动画到关键帧的一半（from 0 ~ 50%).

5. animation-play-state : 允许暂停和回复动画。

6. animation-timing-function : 设置动画速度，建立加速曲线。

7. animation-fill-mode : 指定动画执行前后如何为目标元素引用样式。

8. animation-name : 指定由@keyframes描述的关键帧名称。
		1. none
			特殊关键字，表示无关键帧。可以不改变其他标识符的顺序而使动画失效，或者使层叠的动画样式失效。
		2. IDENT
			标识动画的字符串，由大小写不敏感的字母a-z、数字0-9、下划线(_)和/或横线(-)组成。第一个非横线字符必须是字母，数字不能在字母前面，不允许两个横线出现在开始位置。

###通常可以简写, 简写起来也较为方便

/* @keyframes duration | timing-function | delay |  iteration-count | direction | fill-mode | play-state | name */
	animation: 3s ease-in 1s 2 reverse both paused slidein;

/* @keyframes duration | timing-function | delay | name */
  animation: 3s linear 1s slidein;

/* @keyframes duration | name */
  animation: 3s slidein;