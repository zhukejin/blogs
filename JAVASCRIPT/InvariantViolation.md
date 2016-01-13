React 错误 —— Invariant Violation
--

> 今天遇到这个问题很莫名其妙...

	Uncaught Invariant Violation: <body> tried to unmount. Because of cross-browser quirks it is impossible to unmount some top-level components (eg <html>, <head>, and <body>) reliably and efficiently. To fix this, have a single top-level component that never unmounts render these elements.

看字面意思顶级组件被注销(重新渲染， 就是 body head ， html 这些被干掉了)。

但是不对啊，我规规矩矩在 body 里一个顶级元素 #react 做的所有操作

	<body>
	<div id="react"></div>
	</body>

排查中---


通过排查法找到问题，在我的 jsx 中存在：

	<Col offset='5' style={{display: this.state.currentStep === 2 ? '' : 'none'}}>
        <h3 style={{color: 'rgb(95,188,41)'}}>创建成功<Icon type="check-circle-o" style={{marginLeft: '10px'}} /></h3>
        <p style={{marginBottom: '10px'}}>
            将代码加到网站源文件<body></body>标签之间。推荐结果将通过API接口返回，代码中已包含API调用方法，无需单独配置。
        </p>
        <Input type="textarea" value="请复制这里的代码" disabled style={{marginBottom: '10px', height: '188px', resize: 'none'}}/>
    </Col>

注意里面的 <body> 标签，这个标签被 react 解析了，然后认为我是重新渲染了body 标签.... 

解决方法：

转义此标签为： `&lt;body&gt;&lt;/body&gt;`  即可