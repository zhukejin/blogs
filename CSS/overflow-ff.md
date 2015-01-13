关于 overflow: auto 在ff 中失效的问题
--

>踩过的坑和走过的路

在Table中设置了一个 scroll, 为了没美观， 便加了个  overflow： auto， Chrome 中正常， FireFox 会导致 scroll 失效。


### 解决办法：

在所有的td 中限制一下宽度，
ex： 

	<td  style="min-width: 100px; max-width: 100px;">

