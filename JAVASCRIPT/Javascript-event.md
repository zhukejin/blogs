Javascript 事件冒泡和捕获的一些探讨
--

>本文为笔者实验总结
>
>如有不正，请指点。

_引文：偶然和朋友讨论了事件绑定冒泡和捕获的执行顺序，各持己见，久争不下。
于是实验一番_

此为 HTML 代码：

	<body>
		<ul class="ulclass">
	        <li class="li1class">1</li>
	    </ul>
	</body>

### 实验一： ###

	<script type="text/javascript">
	    var $El = document.querySelector(".ulclass");
	    var $El1 = document.querySelector(".li1class");
	
		//冒泡
	    $El.addEventListener("click",function(){
	        alert("ul");
	    },false);
		//捕获 
	    $El1.addEventListener("click",function(){
	        alert("li");
	    },true);
	</script>

实验分析：冒泡执行过程中是从最内层元素 li 开始往外冒泡，在 经过 li 一层元素后到达 ul 被触发。而捕获过程则一共经历 window-> document -> body -> ul 四层元素后到达 li ，被触发，看起来好像是应该先弹出 ul ，然后再弹出 li。


实验结果：点击 li 内的 1 的时候，第一次弹出 “li”，第二次弹出 “ul” 。

#### 猜想一 ####

- **在冒泡事件和捕获事件同时存在的情况下，捕获事件优先级高一点**。

为了验证这个猜想，再来做几个实验：

### 实验二 ###

	<script type="text/javascript">
	    var $El = document.querySelector(".ulclass");
	    var $El1 = document.querySelector(".li1class");
		//捕获
	    $El.addEventListener("click",function(){
	        alert("ul");
	    },true);

		//冒泡
	    $El1.addEventListener("click",function(){
	        alert("li");
	    },false);
	</script>

实验分析：如果上述猜想是正确的，那么这次的执行结果就应该是先弹出 “ul” 再弹出 “li”。

实验结果：先弹出 “ul” 再弹出 “li”。


### 实验四 ###

再来试一下两个事件都冒泡的情况：

	//冒泡
    $El.addEventListener("click",function(){
        alert("ul");
    },false);

	//冒泡
    $El1.addEventListener("click",function(){
        alert("li");
    },false);

实验分析：如果两个都为冒泡的情况下，点击后应从最里面的元素往外冒，也就是先弹出 li , 再弹出 ul.

实验结果：无误。

然后将两个事件都改成捕获，则先弹出 ul 再 弹出 li ，与预期完全一样。

### 实验五 ###

这里把本来在 ul 上绑定的事件放到 li 上，也就是 li 绑定两个事件，一个为冒泡，一个为捕获。

	var $El = document.querySelector(".ulclass");
    var $El1 = document.querySelector(".li1class");
    //冒泡
    $El1.addEventListener("click",function(){
        alert("冒泡");
    },false);

    //捕获
    $El1.addEventListener("click",function(){
        alert("捕获");
    },true);

实验分析：按照之前的猜测，应该是先弹出捕获，然后弹出冒泡。（如果是这样的话我也不会写这么多了...）
实验结果：先弹出了 冒泡，然后弹出了捕获。

为什么？？

#### 猜想二 

- **在同一个元素的绑定事件中，冒泡和捕获没有次序之分，遵循Javascript的执行顺序。**


#### 实验六 ####

为了进一步证实这个猜测...我们将冒泡和捕获的位置调换一下

	var $El = document.querySelector(".ulclass");
    var $El1 = document.querySelector(".li1class");
    //捕获
    $El1.addEventListener("click",function(){
        alert("捕获");
    },true);

    //冒泡
    $El1.addEventListener("click",function(){
        alert("冒泡");
    },false);

实验结果： 先捕获，后冒泡。

看似是这样了，但是在重复实验五 的时候，li下一级有一个元素 a ，直接点击 li 很符合上面的推测，但是在点击a元素的时候，先弹出了捕获。

为什么？？

#### 猜测三

- **在元素上同时绑定捕获事件和冒泡事件，如果通过此元素的子级元素触发，则优先触发捕获事件，若不通过此元素的子级元素触发，则按照Javascript执行顺序触发。**



附：

冒泡经常用于需要绑定很多事件的时候，给他们父级元素绑定一个事件，可以有效的提高代码执行效率。
比如：

	<body>
    <div class="">
        <ul class="ulclass">
            <li class="li1class">1</li>
            <li class="li1class">1</li>
            <li class="li1class">1</li>
            ...
            ...
            <li class="li1class">1</li>
        </ul>
    </div>
	</body>

假如此处有100个li，每个li元素上都绑定一个事件的话就验证影响了执行效率，那么就可以在ul 上绑定一个事件：

	<script type="text/javascript">
	    var $El = document.querySelector(".ulclass");
	    //冒泡
	    $El.addEventListener("click",function(){
	        console.log(event.target);
	    },false);
	</script>

其中 event.target 就是获取触发来源。
it’ OK。


欢迎指正。