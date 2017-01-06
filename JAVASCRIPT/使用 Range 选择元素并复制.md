使用 Range 选中Dom并进行复制
--
> 本文为作者原创，随时可能修改，为保证准确性，转载请保留本文地址以便读者追本朔源。

### 起因：
JavaScript 选中内容复制的时候，使用 execCommand('selectAll') 是无法选中普通DOM，只能在 "编辑区" 选择.  
参见 [https://developer.mozilla.org/zh-CN/docs/Web/API/Document/execCommand](https://developer.mozilla.org/zh-CN/docs/Web/API/Document/execCommand)

### 方案：

为了简单，不去舍本求末的弄个隐藏域或者 disabled 的文本框，现在创建 Range 来选择。  

1. 创建 Range 

	```
	const range = document.createRange();
	```
	
	> Range表示包含节点和部分文本节点的文档片段。  
	>Range可以用 Document 对象的 createRange方法创建，也可以用Selection对象的getRangeAt方法取得。	另外，可以通过构造函数 Range() 来获得一个 Range 。

2. 选择要复制的元素
 
	```
	const EL = this.el.nativeElement.querySelector('.code');
	```

3. 选择Node

	```
	const selectEl = range.selectNode(EL);
	```
	
	> Range.selectNode()  
	> 设定一个包含节点和节点内容的 Range 
	
4. 将此Node 添加到Range 对象中

	```
	(<any>window).getSelection(selectEl).addRange(range);
	```
	
5. 执行系统复制命令

	```
	document.execCommand('copy');
	```
	
	此时理应是好的，但是实验发现，只有第一次能成功复制，第二次就会报错：

		 Discontiguous selection is not supported.
	 
	翻了翻文档发现每次都需要重置选区，于是加上

6. 重置选区

	```
	window.getSelection().removeAllRanges();
	```	
 即可。

我在Ng2 中完整代码如下：

	private copy() {
        window.getSelection().removeAllRanges();

        const EL = this.el.nativeElement.querySelector('.code');
        const range = document.createRange();
        range.selectNode(EL);

        setTimeout(() => {
            window.getSelection().addRange(range);
            document.execCommand('copy');

            // 防止出现讨厌的黑漆漆的选中,再取消一次
            window.getSelection().removeAllRanges();
        }, 0);
        // range.detach();
        this.btnText = '已复制';
    }
    
    
    
### 兼容性

chrome [✔️]  
Safari [✔️]  
IE [9+]  
Opera[9+]  
Firefox [1.0+]  
    
参考文章：

- [https://developer.mozilla.org/zh-CN/docs/Web/API/Range](https://developer.mozilla.org/zh-CN/docs/Web/API/Range)
- [https://developer.mozilla.org/zh-CN/docs/Web/API/Document/execCommand](https://developer.mozilla.org/zh-CN/docs/Web/API/Document/execCommand)

	



