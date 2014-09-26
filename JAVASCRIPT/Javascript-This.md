七种情况下 This 的指向问题
--
**声明 :**

> 转载请注明本文地址来源，谢谢。
> 
> 本文地址：http://blog.zhukejin.com/archives/215
> 
> email : zhukejin@zhukejin.com

this 是一个动态指针，它指向当前作用域对象，如果当前定义对象的作用域没有发生变化，那么this就会指向当前对象。
This可以存在于任何位置，并不局限与对象、方法内，也可以是其他特殊的上下文环境。

本文实验了下述七种情况：

1. **在函数中的 This**
2. **在构造器中的This**
3. **函数的引用和调用中this的变数**
4. **call 和 apply 对 this的影响**
5. **原型继承中的 This**
6. **异步调用的 This**
7. **定时器中的 This**
8. **被Evel() 方法解析的 This**


### 在函数中的This： 

在实函数中分属两种情况，一种是 Sloppy 模式， 一种是 Strict模式。 [什么是Strict模式?](http://blog.csdn.net/airingyuan/article/details/25036297)

_Sloppy 模式下:_

	(function test() {
		console.log(this) // window
	})()

_Strict 模式下：_

	(function test() {
		'use strict';
		console.log(this) // undefined
	})()



#### 在构造器中的This： 

	var getThis;
	var a = function() {
		getThis = this;
	}
	var b = new a();
	console.log(getThis); // a{}
	console.log(getThis === b); // true

此处通过 new来新建一个对象，并将这个对象通过 this 传入构造器中。

附：Javascript New 的操作原理（[这里是出处](http://speakingjs.com/es5/ch17.html#_the_new_operator_implemented_in_javascript)）：

	function newOperator(Constr, args) {
	    var thisValue = Object.create(Constr.prototype); // (1)
	    var result = Constr.apply(thisValue, args);
	    if (typeof result === 'object' && result !== null) {
	        return result; // (2)
	    }
	    return thisValue;
	}


#### 函数的引用和调用

函数的引用和调用分属两种不同的概念，虽然两者都无法改变函数的定义作用域，但是引用函数可以改变函数的执行作用域，而调用函数是不会改变函数的执行作用域的,如下：

	var o = {
		name: "这是一个对象",
		f   : function () {
			return this;
		}
	}
	
	o.ol = {
		name: "这又是一个对象",
		me  : o.f //引用上个对象的方法
	}

	var who = o.ol.me(); // ol{}

此处this指向的是当前执行域对象 ol， 如果把对象 ol 的 me 属性值改为函数调用：

	o.ol = {
		name : "这又是一个对象",
		me   : o.f() // o
	}

此处this 所代表的是定义函数时所在的作用域对象。


#### call 和 apply ：

call 和 apply 方法能够强制的改变函数的执行作用域，使其作用域指向所传递的参数对象，所以函数中所包含的this关键字也会指向参数对象

	function f() {
		alert(this.x + this.y);
	}

	var o = {
		x : 1,
		y : 2
	}

	f.call(o);

这里其实就通过 call 将函数中的 this指向变为传入的参数对象 o，所以函数中的this.x 也就是 o.x


#### 原型继承：

    function base() {
        this.m = function () {
            return "base";
        };
        this.a = this.m(); //基类的属性a 调用m方法
        this.b = this.m; //基类的属性b 引用m方法
        this.c = function (){ //基类的方法c() ， 以闭包结构调用当前作用域中的m方法
            return this.m();
        }
	}
	function f () {
        this.m = function () {
            return "f";
        }
	}

    f.prototype = new base();
    var ff = new f();
    console.log(ff.a);   // base， 说明this.m() 中this指向 f 的原型对象
    console.log(ff.b()); // base
    console.log(ff.c()); // f ， 说明this.m() 中this指向作用域对象

c为一个闭包体，在子类的实例中调用它时，他的返回值实际上已经成为实例对象的成员，闭包在哪里调用，其中包含的this就会指向哪里。


#### 异步调用：

    var button = document.getElementById('button');
    var o = {};
        o.f = function (){
            if(this == o) console.log("this == o");
            if(this == window) console.log("this == window");
            if(this == button) console.log("this == button");
        }

    button.onclick = o.f;

这里的f() 中的this就不再指向o, 而是指向 button dom 对象。因为是被传递给按钮的事件处理函数之后再被调用的，函数的执行作用域发生了变化。

	//通过事件绑定进行
    if (window.attachEvent)
        button.attachEvent("onclick", o.f);
    else
        button.addEventListener("click", o.f, false)

实验结果是在符合 DOM标准的浏览器中，this 是指向button，但是在 ie 中，this是同时指向 window 和 button，因为attach是 ie的window方法，调用此方法的时候，执行作用域为全局，随后又被注册到按钮对象上，所以this会指向两个目标。

IE上这种情况很可能导致事件绑定的句柄丢失，为了防止这种情况，可以通过call 来强制指定 this 指向：

	button.attachEvent("onclick",function() {
            o.f.call(button); //this 指向button
        });

当然可以强制指向 o 

#### 定时器

异步调用的另一种形式就是使用定时器来调用函数，通过调用 window对象 的 setTimeout() 或者 setInterval() 方法来延期调用函数，例如：

	var o = {};
	o.f = function () {
		if (this == o) console.log("this == o");
		if (this == window) console.log("this == window");
		if (this == button) console.log("this == button");
	}

	setTimeout(o.f, 1000);

程序在IE浏览器下的运行结果同上，this 同时指向 window和 button对象，原理和attachEvent() 一样。

在符合Dom 标准的浏览器中，this指向的是window对象，因为setTimeout() 方法是在全局作用域中被执行的，所以 this 指向window 对象。


### eval () 中的 this：

直接使用eval(), this 指向当前范围

	var x = 'global';
	function directEval() {
	    'use strict';
	    var x = 'local';
	    console.log(eval('x')); // local
	}

通过call () 间接地调用eval(), this 指向全局范围

	var x = 'global';
	function indirectEval() {
	    'use strict';
	    var x = 'local';
	    console.log(eval.call(null, 'x')); // global
	    console.log(window.eval('x')); // global
	    console.log((1, eval)('x')); // global (1)
	
	    var xeval = eval;
	    console.log(xeval('x')); // global
	
	    var obj = { eval: eval };
	    console.log(obj.eval('x')); // global
	}


如有不对，请指正。

Email ： zhukejin@msn.com
