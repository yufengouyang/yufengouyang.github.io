---
title: 记录一下java生产环境CPU占用过高实例
date: 2024-01-03 14:03:37
tags:
- java
categories:
- java基础
---

> 今天还是像往常一样下班后坐公交车回家，突然工作微信群里发来一个截图，我点开一看是我之前上线的服务占用CPU过高了导致程序直接卡死。记录分享一下我的解决思路希望可以帮到你们。

------

### **1. 先查看监控里每个逻辑cpu情况**

```
执行命令：top
```

{% asset_img 0a7ecddcc48f4cddad8cc714895f06f5.png %}

### **2. 查看进程jvm虚拟机堆使用情况**

```
执行命令：jmap -heap 28292
```

{% asset_img 4c1d8f73baac45c99f22b03f45124dea.png %}

### **3. 打印最近一次GC情况**

```
执行命令：jstat -gcutil 28292
```

{% asset_img 8e9af51e348f4b11baa1782e09e589d1.png %}

### **4. 查看java哪个线程cpu占用高**

```
执行命令：top -H -p 28292
```

{% asset_img 83db0396d06543fbbc15a92372470246.png %}

### **5. 确定线程id，再通过命令计算十六进制值**

```
执行命令：printf "%x\n" 28329
```

{% asset_img 9897e2d22d894603bf8a0610b70c86e5.png %}

### **6. 打印该线程堆栈内容**

```
执行命令：jstack 28292 | grep 6ea9 -A 100
```

{% asset_img ff697731db264f39bdd8438201f192c2.png %}

至此，根据截图里面的错误信息基本就可以定义到具体代码类的位置了。
