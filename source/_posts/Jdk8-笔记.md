---
title: Jdk8 笔记一
date: 2018-02-01 14:28:44
categories: "Java进阶"
tags:
	- jdk
---

用法：
```
        List<Apple> apples = new ArrayList<>();
        apples.add(new Apple(100, "red"));
        apples.add(new Apple(20, "yellow"));
        apples.add(new Apple(10, "green"));
        List<Apple> filterList = apples.stream().filter(Apple::isGreen).collect(Collectors.toList());
```
这是一个简单的jdk8中流的操作。将List转化为stream,之后过滤Apple是绿色的输出结果：

> [Apple{weight=100, color='red'}, Apple{weight=20, color='yellow'}, Apple{weight=10, color='green'}]
