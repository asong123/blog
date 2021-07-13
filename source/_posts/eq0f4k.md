---
title: 关于eCharts对数轴以及解决对数轴显示失败只显示1-10区间的问题
urlname: eq0f4k
date: '2021-07-13 19:15:18 +0800'
tags: []
categories: []
---

eCharts 对数轴官方示例：[https://echarts.apache.org/examples/zh/editor.html?c=line-log](https://echarts.apache.org/examples/zh/editor.html?c=line-log)
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1626174956499-87ea0560-8df2-437e-b84a-a8b30e19d5f1.png#clientId=ue5b6dae2-fc20-4&from=paste&id=u0aa46cde&margin=%5Bobject%20Object%5D&originHeight=681&originWidth=1097&originalType=url∶=1&status=done&style=none&taskId=uf77653a1-b9bc-41ef-9192-0a9722703fd)
这种图示挺方便的，对于那种既有大数据，又有小数据，并且差别很大的时候，这种就比较方便，对于很小的数据也能看到趋势。比如有大量数据是 500-1000 之间，还有大量数据在 0.001 - 1 之间。那么不使用对数轴的话，0.001 - 1 之间的数据基本就看不到趋势，使用对数轴的话，就可以看到趋势。
这种就需要设置一下 yAxis 的 type：'log' 即可：
官方配置项手册：[https://echarts.apache.org/zh/option.html#yAxis.type](https://echarts.apache.org/zh/option.html#yAxis.type)

```vue
yAxis: { type: 'log', name: 'y', minorSplitLine: { show: true } }
```

但是存在的问题，就是只要有为 0 的数据，这个对数轴的图就有问题了，只显示 1-10 的区间，如下：
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1626174956608-89ba985b-1523-45bd-8f3e-b15c365399e4.png#clientId=ue5b6dae2-fc20-4&from=paste&id=uf26f1654&margin=%5Bobject%20Object%5D&originHeight=764&originWidth=1872&originalType=url∶=1&status=done&style=none&taskId=u644d2648-3396-43c6-a5a5-49ba8e150fb)
解决方案：就是将 0 的数据，置为空即可。置为空的话，就是中途会中断，如下：
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1626174956629-4012ef40-9a5a-4b0b-9cd7-5974e4a4b5e5.png#clientId=ue5b6dae2-fc20-4&from=paste&id=udb7093cb&margin=%5Bobject%20Object%5D&originHeight=685&originWidth=1854&originalType=url∶=1&status=done&style=none&taskId=ub0cfdc65-0837-4e5e-8b95-a86e15def12)
如果在意上下留白的问题，可以改下对数的基数即可，比如最大数据 500 多，最小数据 88 的话，因为对数默认基数为 10，所以 y 轴会是 1,10,100,1000，导致上下会大量留白，对上的话可以给个最大值设置 max，对下的话就设置下对数基数，让数据大致符合是基数的对数即可。
