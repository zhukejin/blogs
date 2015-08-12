## 前端知识积累

> 因会无定期修改，若需转载请保留本文地址， 为方便读者追本朔源。


### 1.  javascript 中转换数字为 int 可以使用
	
	~~ "42" // 42
	+ "42" //42

### 2. css中， position：absolute， relative；元素会晚于正常文档流元素的渲染， 所以显示层级会比较高

	a:hover {
		position: relative;
	}


### 3. opacity < 1的元素会展示在最上层，所以这样很方便

	.top {
		opacity: .99;
	}


### 4. javascript 删除数组最好通过原型链 xx.length = 0; 而不是 delete 脏删除


### 5. 字符串排序

     '324'.split('').sort().join('');  // 234


### 6. 判断是否为 ie9- 

在 ie 678 中有个神奇的特性

 	window == document //true
	document == window // false


### 7. 我简直是太机智了

如何快速的定义大量的变量：

	[1,2,3,4,5,6,7,8,9,10].map(function (v){
        $scope.selectObj["C"+v+"Tp"] = 'total';
    });

### 8. JAVASCRIPT 获取对象长度

    var a = {a:1, b:2},
        b = Object.keys(a).length;

*tip: 不可枚举的不会出来*


### 9. 如何查找数组中第二大的数值？

    var num = [1,2,3,2,1,8,33,45,1,3,4];

    function findSecondMax(num) {
        var max = Math.max.apply(null,num);
        var arr = [];
        arr=[].filter.call(num,function(i){
        return i!=max;}
        );
        return Math.max.apply(null,arr);
    }
    console.log(findSecondMax(num));

### 10. Javasc获取时间的N种方法：

	new Date().getTime() //最常见的方法， 通过实例化对象调用

	(new Date).getTime() //通过调用值获取， 等同于 ： (new Date().getTime()); Jquery 的 $.now() ， 是封装的这个方法

	
	其他的还有： 

	Date.now()
	
	+new Date

	new Date -0
