iframe 获取父级url
--
> 原创文章， 可能随时更改， 为了方便溯源，转载请保留此行，原文地址：[]()


最近修改 js web数据采集器拿到一个需求：

**当页面被 iframe 引用的时候，需要拿到父级的 url**


首先想到的是 `parent.location.href`, 在本地试了试也没有问题。

但是放服务器上发现， 爆了一个错误 `blocked a frame with origin null`, 

frame 被锁定巴拉巴拉， 无非就是没有权限， iframe 跨域的问题

google 了一下， 改成这样就解决了：

`window.document.referrer`

referrer 不仅可以获得 http- header 的refer， 也能获取 跨域iframe父级页面的url。

*tip：如何判断一个页面是否在 iframe 中打开*

	window.top === window.self

