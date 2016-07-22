如何快速的创建一个包含100个元素的数组
--
> 本文为笔者原创，之后可能随时修改，若需转载请保留此头部，以便读者追本朔源。

> 本文地址 ： [http://blog.zhukejin.com/archives/328](http://blog.zhukejin.com/archives/328) 


> 如题， 最初见到的问题是如何创建100个为元素为0 的数组  
> 研究了一系列的方法，包含Es6 新的API ，不得不说， ES6 好强大！

如果问一个新手，那么得到的回答极有可能就是 **循环**，当然循环也无可厚非，毕竟能实现就行，但是我们如果闲的话，还是可以探讨一下到底有多少中方式可以解决这个问题

- 循环

		var arr = [];
		for (var i=0; i<99; i++) {
			arr.push(0)
    	}

		//可以吗？ 当然可以。 或者使用其他的什么循环

那么有其他方法吗？ 当然有。  
核心围绕在 new Array() 上


- Array构造器

	如上第一个循环的时候，就是用了 new Array(100) 这样的构造器，但是这样创建出来的数组只是一个 undefined x 100的集合

		Array(100) //[undefined × 100]

ECMAScript-262 中对 Array.prototype.map 的方法描述有一句话

`callbackfn is called only for elements of the array which actually exist; it is not called for missing elements of the array.`

在为数组中每个元素执行`callbackfn `之前，会先判断这个元素是否在数组中实际存在（actually exist），判断的方法就是调用内部方法[[HasProperty]]。

不过没关系，我们可以进行一些简单的处理


- split分割

		//创建一个100个元素的数组，此时不具有Array 的 prototype, 然后通过 join 和 split 来生成
		Array(100).join(0).split('')

		//或者
		'0'.repeat(100).split('')

		//或者
		(1 << 24).toString(2).replace(/\d/g, '0000').split('')

		//什么？ 你说这些出来元素都是字符串？要Number ？ 
		Array(100).join(0).split('').map(Number)

		可以了吧
		

- Array.fill

		// Es6 的新方法 Array.prototype.fill
		Array(100).fill(0)

		//tip
		[].fill.call({length: 100}, 0)
	
		//这时候就可以用 Array.from 、 Array.prototype.slice.call 来转换成数组了
		Array(100).fill(0).map(x=>x);

- Array.from + map

		//构造器创建的undefined 集合无法遍历，那么就来转换一下
		Array.from(Array(100)).map(x=>0)

		//或者
		Array.from({length: 100}).map(x=0)

- Array.from + mapFn (from 的第二个参数)

		Array.from({length:100}, x=>0)


- Array.apply + map

		Array.apply(null, Array(100)).map(x=>0)

		//或者
		Array.apply(null,{length:100}).map(x=0)

- 类型化数组

		//它的元素默认初始化为0。
		new Int8Array(100);

		//甚至
		Array.prototype.slice.call(new Uint8Array(100))

- Spread  拓展运算符

		[...Array(100)].map(x=0);

### 延伸一下，如果是求每个元素是他的下标 （部分来自知乎）


- Object.keys

		Object.keys(Array.apply(null,{length:100}));

		//笔者补充，其实可以这样
		Object.keys(new Int8Array(100))

		//或者这样
		Object.keys(String(Array(101)));

		//甚至这样
		Object.keys('0'.repeat(100))

- Array.from + keys

		Array.from(Array(100).keys()) 

- Spread + keys

		[...Array(100).keys()]

- Array.from + Generator

		function* angry(i) {
		  yield i;
		  if (i < 99) { yield* angry(i + 1); }
		};
		Array.from(angry(0));

- 变异递归

		(function(v,i){if(i>0)arguments.callee(v,i-1);v.push(i);return v})([],99);

- 奇葩一点的方法：

		Number.prototype[Symbol.iterator] = function() {
		    return {
		        v: 0,
		        e: this,
		        next() {
		            return {
		                value: this.v++,
		                done: this.v > this.e
		            }
		        }
		    }
		}

		[...100]



- Y combinator （别问，知乎大神写的）

		(function (excited) {
		   return function (f) { 
		      return f(f); 
		   }(function (f) {
		      return excited(function (x) { return (f(f))(x); });
		   });
		})(function (excited) {
		   return function(i) {
		       return (i < 0) ? [] : excited(i - 1).concat(i);
		   }
		})(99);


		//换个装逼点的es6 

		((excited) => ((f) => f(f))((f) => excited((x) => (f(f))(x))))((excited) => (i) => (i<0 ? [] : excited(i-1).concat(i)))(99)



**写在最后**

可能你看到这里会觉得内心毫无波动，甚至有一点想笑：“妈哒智障，研究这些东西干什么，会用循环就够了”  
但是基于我的求知欲，看到这些东西就有兴趣就追根问底，而且在写这篇文章的时候，确实安利到不少新的黑科技。  
也巩固了一遍 Array.from ， Object.keys 这些Es6 的新API  
甚至 Y combinator 这样的黑魔法。 

**另外**
  
如果有不对的，漏的，请补充。