# Javascript 变量作用域
 
### 那些隐藏起来的陷阱....


>本文地址：[http://blog.zhukejin.com/archives/233](http://blog.zhukejin.com/archives/233)
>
>原创文章， 之后可能随时更新， 为了更好追溯，请转载时保留地址

>如何联系我？ [给我发邮件](mailto:zhukejin@msn.com)


首先大家都知道的, Javascript 变量作用域比较特殊，如果在一个函数中赋值一个临时变量， 那么外界是无法读取的。

ex：

	var a;
	(function () {
		var a = 1;
	})()
	
	console.log(a); // JS 报错 ， a is undefined


php 中就可以这样， <del>为啥Js 没有传引用啊</del>

	$a = 0;
	function test(&$a) { //传引用...
	    $a = 1;
	}
	test($a);
	echo $a;


*小 tip： Javascript 函数传值中， 对象和数组是传引用の*

ex：

	var s = {name:"a"};
	function a(obj){
	 	obj.name="b";
	}
	a(s);
	console.log(s); // Object {name: "b"}

**好像跑题了？**  其实是为了 <del>凑字数</del> 发散思维 o(>﹏<)o


好吧， 其实我之前说的陷阱是这样的：

	function test(){
	  var a = b = 1;
	}
	
	test();
	//console.log(a); //error
	console.log(b); //1

看起来 a & b 都是赋值为临时变量， 但是其实 b 是全局变量。。。(有点坑)

那么赋值语句可以这么改..
	
	var a = 1, b = 1;

这样的话， a 和 b 都是局部变量了。

**那么换一个方式**： （ex2）

	var a = 1;
	function test () {
	  console.log(a); //undefined
	  var a = 2;
	}
	test();
	console.log(a); // 2

这里涉及一个 **声明提前** 的问题， 上述 function 内代码相当于：

	var a;
	a = 2;

变量和函数的声明都被提前至函数体的顶部，而同时变量并没有被赋值。(函数是可以运行的)

	

如果这样呢， 

	var a = 1;
	function test (a) {
	  console.log(a); //1
	  a = 2;
	}
	test(a);
	console.log(a); // 1

这里为什么会输出两个1？

原因在这里： **Javascript在执行前会对整个脚本文件的声明部分做完整分析(包括局部变量)，从而确定实变量的作用域， a 其实是  a = arguments[0], 这个时候接收到外部的传参， 当全局变量跟局部变量重名时，局部变量的scope会覆盖掉全局变量的scope。所以在funtin 中的 a 其实已经不是外部声明的 a 了。（至少在作用域上讲， 已经不是一个a了）**。

如果想在重名时使用全局变量， 这样...

	var a = 1;
	function test (a) {
	  console.log(a); //1
	  window.a = 2;
	}
	test(a);
	console.log(a); // 2
