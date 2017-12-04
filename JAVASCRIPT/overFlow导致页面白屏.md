OverFlow 导致页面白屏
--
> 因可能随时修改，所以转载请注明来源，以便读者追本朔源  
> 本文地址: [http://blog.zhukejin.com/archives/368](http://blog.zhukejin.com/archives/368)

### 现象
- 系统：iOS 11 + Safari
- 框架：React + redux + react-router + Ant Design Mobile

#### 复现过程

在组件中使用了 antd 的 picker （time picker、date picker、select picker等等任意picker）后，点击某链接进入其他组件，会导致页面白屏。  

该问题只出现在ios 中， PC 端 chrome 模拟正常。

使用 XCode 安装的ios 模拟器也会出现这个问题。

#### 探索

通过 Safari debug 发现
 
- 白屏后元素还在，css 属性也都很正常
- react 的 render 方法也执行了，didMount 也正常执行
- Picker 曾给 body 添加了一个 overflow: hidden; 的属性，但是在关闭的是马上又删除了。此举是防止picker 打开后body 滚动的问题。
- 在body 上任意设置 overflow 属性或者 display 属性，内容立即就显示出来了。


#### 猜测

由于之前在 firefox PC 上遇到过类似的问题，所以猜测是浏览器渲染的问题。

**可能** 浏览器在执行某一步的时候 *跳过/卡住* 等等，导致浏览器没有正确的渲染DOM。
具体原因还有待探索

#### 解决方案

```
// todo 暂时可以解决其他组件打开picker 后首页不渲染的问题，但是需要寻找真正原因
componentDidMount() {
    delay(() => {
        window.document.body.style.overflowY = 'hidden';
        delay(() => {
            window.document.body.style.overflowY = 'auto';
        }, 0)
    }, 0);
}
```