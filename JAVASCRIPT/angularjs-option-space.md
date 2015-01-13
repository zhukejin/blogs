angularjs select 循环中出现第一个 option 为空格问题
--

> 原创文章， 可能随时更改， 为了方便溯源，转载请保留此行：[]()


刚刚上手 NG，难免出现很多小白问题，老手请绕道。

angular双向绑定的设定让人耳目一新， 其中指令更是极为强大，生成一个select 筛选框只需要：

	ng-option="option.key as option.value for option in optionList"

这样的格式即可。

select DOM 中需要一个默认值 `ng-model`， 去选定option 生成的元素

如果 ng-model 为空， 那么就没有默认选择的 option ， 生成下拉框的时候就多生成了一个空行：

	<option value='?'><option>
	<option value='0'>aa<option>
	<option value='1'>bb<option>

如此这般。

知道了原因， 解决办法就很简单了， 可以设置一个默认值， 可以设置一个默认 option

比如 
	
	$scope.optionList.push({'all': '全部'})

比如

	<option>全部</option>

即可解决。