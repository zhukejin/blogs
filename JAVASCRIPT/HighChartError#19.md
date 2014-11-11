关于 highchart error #19
--

> 踩过的坑和走过的路
> 
> 本文地址

>转载请注明

最近业务上需要画一些表格，使用了HighChart，总体上还不错，但是总有一些坑需要一个个的去踩...

今天在画一个21个指标的折线图的时候遇到了一个问题：首先自定义x轴为类目，

	["201301","201302","201303","201304","201305","201306","201307","201308","201309","201310","201311","201312","201401","201402","201403","201404","201405","201406","201407","201408","201409","201410"]

然后发现报错:

	Highcharts Error #19
	Too many ticks
	This error happens when you try to apply too many ticks to an axis,
	specifically when you add more ticks than the axis pixel length. 
	In practice, it doesn't make sense to add ticks so densely that they can't be distinguished from each other. 
	One cause of the error may be that you set a tickInterval that is too small for the data value range. 
	In general, tickPixelInterval is a better option, as it will handle this automatically. 
	Another case is if you try to set categories on a datetime axis, 
	which will result in Highcharts trying to add one tick on every millisecond since 1970.

这段的意思大概就是说图表上的点太密集了巴拉巴拉，如果太密集看起来不方便就没有作图的意义了巴拉巴拉...

去网上搜索也没有发现有人提出解决方案。

从报错字面意思上分析，是由于时间点过于密集而发生错误，于是我就去注释掉所有与时间有关的代码，在不停的尝试修改各个地方代码后终于发现原因所在：

	...
	...
	plotOptions: {
            series: {
            pointStart: Date.UTC(<?php echo $pointStart; ?>),
                    pointInterval: 24 * 3600 * 1000, // one day
                    events:{

	...
	...


这段代码存在的意义在于之前计算x轴为默认时间点开始后每一天时间为一个点

但是如果自定义了类目后，就出现了 #19...

具体详情由于时间问题没有深究，先留一坑站个位置，以后慢慢研究。

2014-11-11 15:02:03