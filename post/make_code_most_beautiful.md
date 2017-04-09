+++
date = "2017-02-19"
title = "怎样才能写出绝对的代码"
draft = false
description = ""
tags        = ["程序员"]
categories = [ "技术"]
+++

怎样写出无可指摘的代码
<!--more-->

## 什么是绝对的代码

任何人见到这套代码, 都不会说这个架构设计的不好, 这个代码还可以再优化一下. 最多可以说用另外一套架构可以完成同样的功能, 可以有另外的优点, 但是代价是牺牲当前这套架构的一些优点, 换句话说, 当前的代码实现了指定的功能, 具有某些优点, 已经没法子再改进, 一改进就是另外一套东西了

## 具体方法

### 1. 目标清晰具体

目标清晰的意义在于划定功能的边界, 边界确定的清晰, 功能代码就越清晰. 如果目标模棱两可, 那后人维护的时候就会以为某个功能做得不够全面, 而实际上是对功能的理解偏差

### 2. 思路简单到极致

没有人可以通过将功能变得复杂而使得代码让人无可指摘, 总可以通过添加一些代码实现新的功能. 只有在精简到极致的情况下, 才有可能让人无可再减, 也就无可指摘

### 3. 架构清晰

架构清晰的意义在于表现思路的简单. 模块组合简单合理, 没有冗余的模块, , 功能实现层次分明, 具有必须的模块和层次, 没有多余的层次抽象, 每个人都能看得懂, 最简单的做法就是按照当前的思路去实现.

### 4. 清晰的函数

每个函数都是独立的, 每个函数的命名都很清晰, 每个函数的功能都很具体, 函数这些构成系统的细胞都很简单明确, 然后优美的组合成模块, 保证了系统底层的简单干净