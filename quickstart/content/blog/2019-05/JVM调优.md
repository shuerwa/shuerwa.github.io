---
date: "2019-05-10T10:23:48+08:00"
publishdate: "2019-05-10"
lastmod: "2019-05-10"
draft: false
title: "JVM调优"
tags: ["Java"]
series: ["JVM"]
categories: ["Java基础"]
img: "images/blog/2018-08/eveningSun.png"
toc: true
---

## 原则
1. 多数的Java应用不需要在服务器上进行GC优化； 
2. 多数导致GC问题的Java应用，都不是因为我们参数设置错误，而是代码问题； 
3. 在应用上线之前，先考虑将机器的JVM参数设置到最优（最适合）；
4. 减少创建对象的数量；
5. 减少使用全局变量和大对象；
6. GC优化是到最后不得已才采用的手段；
7. 在实际使用中，分析GC情况优化代码比优化GC参数要多得多；
#### 目的
- 将转移到老年代的对象数量降低到最小 **（降低full GC次数）**；
- 减少full GC的执行时间；
#### 做法
- 减少使用全局变量和大对象；**原因：因为大对象和全局变量是直接放入年老代的，使用过多会加速年老代内存空间的消耗**
- 调整年轻代的大小到最合适（如只使用 -xmn700m 则年轻代的最大内存和最小内存都为700m）；**原因：防止minor GC后的堆抖动**
- 设置年老代的大小为最合适；**原因同上**
- 选择合适的GC收集器 **如：java1.7 默认使用 ParNew（年轻代）+CMS（年老代）+Serial Old（年老代，用于处理CMS发生Concurrent Mode Failure的情况）；即 -XX:+UseConcMarkSweepGC**   

**常用JVM观察工具** jconsle 目录 JDK/bin VisualVM

*注：Concurrent Mode Failure 发生在CMS GC的过程中，又有对象需要放到old区，但是old区空间不足了，放不下的情况就会发生此错误*  
-[1] 详情见链接 https://www.cnblogs.com/technologykai/articles/8986236.html
## 版本控制

| Version | Action            | Time       |
| ------- | ----------------- | ---------- |
| 1.0     | Init 从有道云笔记移出 | 2019-05-10 |

