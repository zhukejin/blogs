ES5 Array|Object 方法实践及扩展
--

>Author: 凤歌  
>本文地址： <a id="selfUrl" href=""><script type="text/javascript">document.write(window.location.href);
document.querySelector('#selfUrl').setAttribute('href', window.location.href)
</script></a>  
> 本文所述皆为作者整合互联网资料所得，不保证其准确性，因此会不定期修改  
> 为方便读者追本朔源， 请转载者保留此行。


首先来讲一个小技巧， 每次写博文都要先发布， 然后拿到 url ，觉得十分麻烦。 于是就写了一个自动获取本页 url 的标签：

    <a id="selfUrl" href=""><script type="text/javascript">
        var url = window.location.href;
        document.write(url);
        document.querySelector('#selfUrl').setAttribute('href', url);
    </script></a>
当然，这个只是本文的附送，下面进入正题。

ES5 (ECMA-262 5.1 规范)是2011年被采纳的，但是直到现在还有很多人在使用旧版本的 API。

###Array：

1. **map:**  
这个方法也是我平时使用最多的一个方法，创建循环啊， 或者循环一些其他数组啊，真的是方便极了。  
类似于后端中的 map() ，也如字面意思“映射” ， map 是创建一个新的映射数组，然后计算完毕后返回结果。  
来看例子， 需求： 需要创建一个对象， 此对象中含有10个可区分，可枚举的元素。  

	例子1：

		var local = {};
		[1,2,3,4,5,6,7,8,9,10].map(function (v){
        	local['hehe' + v] = Math.random();
    	})
    	console.log(local) // 

	这样就循环创建需求的对象...  


	例子2：  

		var arr = [1,2,3,4,5];
	    var arr1 = arr.map(function(v){
	        return v * 2;
	    });
	    console.log(arr) //[1,2,3,4,5]
	    console.log(arr1) //[2,4,6,8,10]

	可以看出改变的只是映射数组。    
	让IE6 ~ IE8 支持 map :

		if (typeof Array.prototype.map != "function") {
		  Array.prototype.map = function (fn, context) {
		    var arr = [];
		    if (typeof fn === "function") {
		      for (var k = 0, length = this.length; k < length; k++) {      
		         arr.push(fn.call(context, this[k], k, this));
		      }
		    }
		    return arr;
		  };
		}


	当然，这仅仅是举个例子而已， 具体使用方法当然还是变化无穷的，招式只有一种， 但是却可以衍生出无数种变化。

2. **forEach：**  
forEach的使用方法和使用情况类似于 map ，刚刚的例子用 forEach 同样可以做到，只不过和 map 明显的区别是 map直接对整个列表数据进行函数操作， 而foreach是迭代对每行数据进行逐一操作  

	例子：

		var arr = [1,2,3,4,5];
	    var arr1 = arr.forEach(function(v){
	        return v * 2;
	    });
	    console.log(arr) //[1,2,3,4,5]
	    console.log(arr1) // undefined

	  同样是循环， forEach 并没有返回值，他是一个 arr-> 空 的映射。  	
	扩展低版本浏览器:


		if (typeof Array.prototype.forEach != "function") {
		  Array.prototype.forEach = function (fn, context) {
		    for (var k = 0, length = this.length; k < length; k++) {
		      if (typeof fn === "function" && Object.prototype.hasOwnProperty.call(this, k)) {
		        fn.call(context, this[k], k, this);
		      }
		    }
		  };
		}

	forEach 和 上面的 map 一样都是接收一个参数， 拥有一个默认的传参， 可以把 alert 函数传进去试一试：

		[1,2,3,4].forEach(alert) // 分别弹出 1 ，2 ，3， 4

	同理可以传进去 console.log ， 但是有一点需要注意的是， console.log 是在console 对象下的一个方法， 和 alert 不同的是， 它需要他的 this 必须 指向 console。 
	这里 forEach , map 都支持第二个参数， 用来修改上下文的 this 关系。

		[1,2,3,4].forEach(console.log, console); // 。。。

	不然就是报 Illegal invocation... 


3. **filter:**  
过滤方法，用法和上面两个差不多。  

		var a = [true, false, 0, 1, "test",3];
	    var b = a.filter(function(v){
	        return v;
	    });
	
	    console.log(b) //[true, 1, "test", 3]

	过滤掉数组中为 == false 的元素。第一眼看你或许觉得很鸡肋.. 但是可以配合一些判断来return ， 就很强大了 。  
	比如通过正则来匹配？

4. **some:**  
检查(并要求部分匹配)数组中符合条件的所有元素：

		var a = [1,2,3,4,5];
	    var c = 3;
	    var b = a.some(function(v){
	        return v > c;
	    });
	
	    console.log(b) // true

	只要 a 中有一个元素是符合条件的 ， 那么 b 就是 true

5. **every:**  
检查(并要求完全匹配)数组中的所有元素:

		var a = [1,2,3,4,5];
	    var c = 3;
	    var b = a.every(function(v){
	        return v > c;
	    });
	
	    console.log(b) // false

	同样的例子， 只是这里需要全部符合给出的条件， 所以这里返回 false， 如果把原始数组变成 : var a = [4,5];  
那么就返回  true了。

6. **indexOf:**  
等同于 字符串中的 indexOf:

		var a = [4,5];
	    console.log(a.indexOf(4)) //0
	    console.log(a.indexOf(0)) //-1

7. **lastIndexOf**  
与indexOf 类似， 不过是逆向查找


8. **reduce**  
此方法接收四个参数： 迭代前一个的值， 此间的值， 索引值， 数组本身，我们来计算一下 1 +2 +3 +... + 100； 

		//传统的循环计算
	    var b = 0;
	    for (var i = 1; i < 101; i++) {
	        b += i;
	    };
	    console.log(b) //5050

		//高级一点的递归计算
	    function c(i) {
	        if (i===0) return 0;
	        return i+c(i-1);
	    }
	    console.log(c(100)) //5050

		//es5 reduce 方法, 因为是数组方法， 这里为了演示， 多此一举的来做个 for 循环来生成数组。
	    var l = [];
	    for (var i = 1; i < 101; i++) {
	        l.push(i);
	    };
	    var c = l.reduce(function (last, now, index, array){
	        return last + now;
	    });
	
	    console.log(c) //5050


		// 二维数组扁平化
	    var matrix = [[1, 2], [3, 4], [5, 6] ];
	    var flatten = matrix.reduce(function (previous, current) {
	      return previous.concat(current);
	    });
	
	    console.log(flatten); // [1, 2, 3, 4, 5, 6]

9. **reduceRight**  
同上， 不过是从右往左。

### Object:

1. **freeze**  
锁定对象，使之无法被修改：

		var a = {a:1};
		a.a  = 4
		console.log(a.a); // 4
		
		Object.freeze(a);
		a.a = 8;
		console.log(a.a); // 4

	可以使用 Object.isFrozen 来判断是否 被锁定

		Object.isFrozen(a) //true

2. **seal**  
锁定元素，可以修改，但是无法删除和写入：

		var a = {a:1};
		Object.seal(a);
		a.a=2
		console.log(a); //Object {a: 2}
		delete a.a // false
		console.log(a) //Object {a: 2}
		a.b = 3;
		console.log(a) //Object {a: 2}

	可以使用 Object.isSealed(a) 来判断对象是否被锁定元素

		Object.isSealed(a) //true

3. **preventExtensions**  
锁定对象属性，可以修改和删除， 但是无法写入

		var a = {a:1}
		Object.preventExtensions(a)
		a.b = 1
		console.log(a) //Object {a: 1}
		a.a = 2
		console.log(a) //Object {a: 2}
		delete a.a
		console.log(a) //Object {}
		
	可以使用 Object.isExtensible() 来判断一个元素是否被锁定属性

		Object.isExtensible(a) //true

4. **keys**  
取到object中所有的键名，类似于 PHP 中的array_keys , 但是接收一个 [callback], 返回一个数组

		var a = {a:1 , b:2};
		Object.keys(a); //["a", "b"]
	通过这个特性， 可以做一个其他判断.. 比如检查是不是一个空对象， 或者检查对象里有几个元素。
	传统的做法是使用  for in 循环 (JQuery 也是这个做的)

		
		var a = {a:1, b:3}, _c = false;
		for (x in a) {
		    console.log(x)
		    return true;
		} // a b

	这样如果返回是true ，就说明进入了 for in 循环， 证明不是一个空对象。   
	那么使用新的 keys 方法呢？

		Object.keys(a)// []
	因为是一个数组， 所以可以直接使用 .length 计数.. 484 很方便啊... 但是有一点需要注意的是， Object.keys 只能遍历出 所有 **可枚举** 的属性。
	
	我们来创建一个 不可枚举的属性。

		Object.defineProperty(a, 'test', {value:123, enumerable : false});
		console.log(a) //{a:1, b:3, test:123}
		Object.keys(a); // ["a", "b"]

	keys 方法并没有把 test这个属性拿出来， 如果想拿到不可枚举的 呢？ 看下面。

5. **getOwnPropertyNames**  
取到所有 属性名，类似于 keys . 但是可以拿到不可枚举的属性名， 但是拿不到 prototy 中的属性

		var a = {a:1}
		Object.defineProperty(a, 'test', {value:123, enumerable : false});
		Object.getOwnPropertyNames(a) // ["a", "test"]

	我们来设置一个 prototy 属性：

		Object.defineProperty(a, "hehe", {
			value: 12,
			writable : false
		})

		Object.getOwnPropertyNames(a) // ["a", "test"]

	怎么拿到 prototy 中的属性呢？

		Object.getOwnPropertyDescriptor(a, "hehe")；
		// 结果为： Object {value: 22, writable: false, enumerable: false, configurable: false}



6. **defineProperty**  
前面已经用了两次了... 就是创建 prototy 属性， 有四个参数可以设置：

	1. value：值，默认是undefined
	2. writable：是否是只读property，默认是false,有点像C#中的const
	3. enumerable：是否可以被枚举(for in)，默认false
	4. configurable：是否可以被删除，默认false

7. **create**  
创建一个对象 `Object.create(prototype[,descriptors])`,并把其prototype属性赋值为第一个参数，同时可以设置多个descriptors

		var o = Object.create({
            "say": function () {
                alert(this.name);
            },
            "name":"Byron"
        });
	
		// res
		Object {say: function, name: "Byron"}
			__proto__: Object
			name: "Byron"
			say: function () {
			arguments: null
			caller: null
			length: 0
			name: ""
			prototype: Object.create.say
			__proto__: function Empty() {}
			<function scope>
			__proto__: Object