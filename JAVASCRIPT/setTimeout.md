关于 setTimeout() 方法的一些用法
--

>部分内容引自 pandacafe.net
>原文地址：http://pandacafe.net/blog/337


引：之前在技术群里有人提起这个问题，就查阅了上述boke，一一写了代码测试了下，结果真发现有问题。

	<input type="text" onkeydown="show(this.value)">
	<div></div>
	<script type="text/javascript">
  		function show(val) {
    	document.getElementsByTagName('div')[0].innerHTML = val;
  		}
	</script>

这段代码中，当用户按下某一个按键，keydown会捕获这个按下动作，然后将按键代码解释后填充到dom中，但是实际上用了发现总有一个延迟，当按下第二个键的时候，第一个键位的值才打印到页面中，why？

原因很简单，当用户按下按键的时候，JavaScript 引擎需要执行 keydown 的事件处理程序，然后更新文本框的 value 值，这两件事也需要按顺序来，事件处理程序执行时，更新 value 值的任务则进入队列等待。所以我们在 keydown 的事件处理程序里是无法得到更新后的 value 的。

怎么解决这个问题呢？

	<input type="text" onkeydown="var self=this; setTimeout(function() {show(self.value)}, 0)">
	<div></div>
	<script type="text/javascript">
	  function show(val) {
	    document.getElementsByTagName('div')[0].innerHTML = val;
	  }
	</script>

上边代码使用的 setTimeout() 方法将 keydown 的填充阶段提前放入执行队列。

JavaScript 是单线程执行的，也就是无法同时执行多段代码，当某一段代码正在执行的时候，所有后续的任务都必须等待，形成一个队列，一旦当前任务执行完毕，再从队列中取出下一个任务。这也常被称为 “阻塞式执行”。所以一次鼠标点击，或是计时器到达时间点，或是 Ajax 请求完成触发了回调函数，这些事件处理程序或回调函数都不会立即运行，而是立即排队，一旦线程有空闲就执行。假如当前 JavaScript 进程正在执行一段很耗时的代码，此时发生了一次鼠标点击，那么事件处理程序就被阻塞，用户也无法立即看到反馈，事件处理程序会被放入任务队列，直到前面的代码结束以后才会开始执行。如果代码中设定了一个 setTimeout，那么浏览器便会在合适的时间，将代码插入任务队列，如果这个时间设为 0，就代表立即插入队列，但不是立即执行，仍然要等待前面代码执行完毕。所以 setTimeout 并不能保证执行的时间，是否及时执行取决于 JavaScript 线程是拥挤还是空闲。


一个简单的例子：

	function a() {
	  setTimeout( function(){
	    alert(1)
	  }, 0);
	  alert(2);
	}
	a();


这里setTimeout()设置的时间为 0 ，看似会不起作用，但是输出的时候会先执行 alert(2)，再执行 alert(1).


