Javascript 奇技淫巧
--
>原文： http://modernweb.com/2013/12/23/45-useful-javascript-tips-tricks-and-best-practices/

>作者： Saad Mousliki

>JavaScript是一个绝冠全球的编程语言，可用于Web开发、移动应用开发（PhoneGap、Appcelerator）、服务器端开发（Node.js和Wakanda）等等。JavaScript还是很多新手踏入编程世界的第一个语言。既可以用来显示浏览器中的简单提示框，也可以通过nodebot或nodruino来控制机器人。能够编写结构清晰、性能高效的JavaScript代码的开发人员，现如今已成了招聘市场最受追捧的人。


### 从数组中随机获取成员

	var items = [12, 548 , 'a' , 2 , 5478 , 'foo' , 8852, , 'Doe' , 2145 , 119];
	var  randomItem = items[Math.floor(Math.random() * items.length)];

### 获取指定范围内的随机数

	var x = Math.floor(Math.random() * (max - min + 1)) + min;

### 生成从0到指定值的数字数组

	var numbersArray = [] , max = 100;
	for( var i=1; numbersArray.push(i++) < max;);  // numbers = [1,2,3 ... 100]

### 生成随机的字母数字字符串

	function generateRandomAlphaNum(len) {
	    var rdmString = "";
	    for( ; rdmString.length < len; rdmString  += Math.random().toString(36).substr(2));
	    return  rdmString.substr(0, len);
	}

### 打乱数字数组的顺序

	var numbers = [5, 458 , 120 , -215 , 228 , 400 , 122205, -85411];
	numbers = numbers.sort(function(){ return Math.random() - 0.5});

### 字符串去空格

	String.prototype.trim = function(){return this.replace(/^\s+|\s+$/g, "");};

### 数组之间追加

	var array1 = [12 , "foo" , {name "Joe"} , -2458];
	var array2 = ["Doe" , 555 , 100];
	Array.prototype.push.apply(array1, array2);

### 对象转换为数组

	var argArray = Array.prototype.slice.call(arguments);

### 验证是否是数字

	function isNumber(n){
	    return !isNaN(parseFloat(n)) && isFinite(n);
	}

### 验证是否是数组

	function isArray(obj){
	    return Object.prototype.toString.call(obj) === '[object Array]' ;
	}

如果 object.toString 已经被重写，那么用这个：

	Array.isArray(obj);

### 获取数组中的最大值和最小值

	var  numbers = [5, 458 , 120 , -215 , 228 , 400 , 122205, -85411]; 
	var maxInNumbers = Math.max.apply(Math, numbers); 
	var minInNumbers = Math.min.apply(Math, numbers);

### 不要直接从数组中delete或remove元素

如果对数组元素直接使用delete，其实并没有删除，只是将元素置为了undefined。数组元素删除应使用splice。
	
	var items = [12, 548 ,'a' , 2 , 5478 , 'foo' , 8852, , 'Doe' ,2154 , 119 ]; 
	items.length; // return 11 
	delete items[3]; // return true 
	items.length; // return 11 
	/* items 结果为 [12, 548, "a", undefined × 1, 5478, "foo", 8852, undefined × 1, "Doe", 2154, 119] */

上述用 delete ， 没有彻底删除内容，应用splice：

	var items = [12, 548 ,'a' , 2 , 5478 , 'foo' , 8852, , 'Doe' ,2154 , 119 ]; 
	items.length; // return 11 
	items.splice(3,1) ; 
	items.length; // return 10 
	/* items 结果为 [12, 548, "a", 5478, "foo", 8852, undefined × 1, "Doe", 2154, 119]

删除对象的属性时可以使用delete。

### 使用length属性截断数组

	var myArray = [12 , 222 , 1000 , 124 , 98 , 10 ];  
	myArray.length = 4; // myArray = [12 , 222 , 1000 , 124].

### 使得map()函数方法对数据循环

	var squares = [1,2,3,4].map(function (val) {  
	    return val * val;  
	});

### 保留指定小数位数

	var num =2.443242342;
	num = num.toFixed(4);  // num = 2.4432

toFixec()返回的是字符串

### 浮点计算的问题

	0.1 + 0.2 === 0.3 // is false 
	9007199254740992 + 1 //  9007199254740992
	9007199254740992 + 2 //  9007199254740994

JavaScript的数字都遵循IEEE 754标准构建，在内部都是64位浮点小数表示。可以通过使用toFixed()和toPrecision()来解决这个问题。

### 通过for-in循环检查对象的属性

	for (var name in object) {  
	    if (object.hasOwnProperty(name)) { 
	        // do something with name
	    }  
	}

### 逗号运算符

	var a = 0; 
	var b = ( a++, 99 ); 
	console.log(a);  // a = 1 
	console.log(b);  // b = 99

逗号运算符在JavaScript在的优先级是最底的


### 提前检查传入isFinite()的参数

	isFinite(0/0) ; // false
	isFinite("foo"); // false
	isFinite("10"); // true
	isFinite(10);   // true
	isFinite(undefined);  // false
	isFinite();   // false
	isFinite(null);  // true，这点当特别注意、

### 用JSON来序列化与反序列化

	var person = {name :'Saad', age : 26, department : {ID : 15, name : "R&D"} };
	var stringFromPerson = JSON.stringify(person);
	/* stringFromPerson 结果为 "{"name":"Saad","age":26,"department":{"ID":15,"name":"R&D"}}"   */
	var personFromString = JSON.parse(stringFromPerson);
	/* personFromString 的值与 person 对象相同  */

### 不要使用eval()或者函数构造器

eval()和函数构造器（Function consturctor）的开销较大，每次调用，JavaScript引擎都要将源代码转换为可执行的代码。

	var func1 = new Function(functionCode);
	var func2 = eval(functionCode);

### 避免使用with()

使用with()可以把变量加入到全局作用域中，因此，如果有其它的同名变量，一来容易混淆，二来值也会被覆盖

### 不要对数组使用for-in

避免：

	var sum = 0;  
	for (var i in arrayNumbers) {  
	    sum += arrayNumbers[i];  
	}

使用：

	var sum = 0;  
	for (var i = 0, len = arrayNumbers.length; i < len; i++) {  
	    sum += arrayNumbers[i];  
	}

另外一个好处是，i和len两个变量是在for循环的第一个声明中，二者只会初始化一次，这要比下面这种写法快：

	for (var i = 0; i < arrayNumbers.length; i++)

### 传给setInterval()和setTimeout()时使用函数而不是字符串

如果传给setTimeout()和setInterval()一个字符串，他们将会用类似于eval方式进行转换，这肯定会要慢些，因此不要使用：

	setInterval('doSomethingPeriodically()', 1000);  
	setTimeout('doSomethingAfterFiveSeconds()', 5000);

而是用：

	setInterval(doSomethingPeriodically, 1000);  
	setTimeout(doSomethingAfterFiveSeconds, 5000);

### 使用对象作为对象的原型

	function clone(object) {  
	    function OneShotConstructor(){}; 
	    OneShotConstructor.prototype = object;  
	    return new OneShotConstructor(); 
	} 
	clone(Array).prototype ;  // []

### HTML字段转换函数

	function escapeHTML(text) {  
	    var replacements= {"<": "<", ">": ">","&": "&", "\"": """};                      
	    return text.replace(/[<>&"]/g, function(character) {  
	        return replacements[character];  
	    }); 
	}


### 不要在循环内部使用try-catch-finally

try-catch-finally中catch部分在执行时会将异常赋给一个变量，这个变量会被构建成一个运行时作用域内的新的变量。
切忌：

	var object = ['foo', 'bar'], i;  
	for (i = 0, len = object.length; i <len; i++) {  
	    try {  
	        // do something that throws an exception 
	    }  
	    catch (e) {   
	        // handle exception  
	    } 
	}

应该：

	var object = ['foo', 'bar'], i;  
	try { 
	    for (i = 0, len = object.length; i <len; i++) {  
	        // do something that throws an exception 
	    } 
	} 
	catch (e) {   
	    // handle exception  
	}


### 使用XMLHttpRequests时注意设置超时

XMLHttpRequests在执行时，当长时间没有响应（如出现网络问题等）时，应该中止掉连接，可以通过setTimeout()来完成这个工作：

	var xhr = new XMLHttpRequest (); 
	xhr.onreadystatechange = function () {  
	    if (this.readyState == 4) {  
	        clearTimeout(timeout);  
	        // do something with response data 
	    }  
	}  
	var timeout = setTimeout( function () {  
	    xhr.abort(); // call error callback  
	}, 60*1000 /* timeout after a minute */ ); 
	xhr.open('GET', url, true);  
	xhr.send();

同时需要注意的是，不要同时发起多个XMLHttpRequests请求。


### 处理WebSocket的超时

通常情况下，WebSocket连接创建后，如果30秒内没有任何活动，服务器端会对连接进行超时处理，防火墙也可以对单位周期没有活动的连接进行超时处理。

为了防止这种情况的发生，可以每隔一定时间，往服务器发送一条空的消息。可以通过下面这两个函数来实现这个需求，一个用于使连接保持活动状态，另一个专门用于结束这个状态。

	var timerID = 0; 
	function keepAlive() { 
	    var timeout = 15000;  
	    if (webSocket.readyState == webSocket.OPEN) {  
	        webSocket.send('');  
	    }  
	    timerId = setTimeout(keepAlive, timeout);  
	}  
	function cancelKeepAlive() {  
	    if (timerId) {  
	        cancelTimeout(timerId);  
	    }  
	}

keepAlive()函数可以放在WebSocket连接的onOpen()方法的最后面，cancelKeepAlive()放在onClose()方法的最末尾。

### 原始操作符比函数调用快

一般不要这样：
	
	var min = Math.min(a,b); 
	A.push(v);
可以这样来代替：

	var min = a < b ? a : b; 
	A[A.length] = v;


	