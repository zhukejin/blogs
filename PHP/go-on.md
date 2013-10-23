每天进步一点点
-

#####1.

    <?php $a = 3; $b = 5; if($a = 5 || $b = 7) {  $a++;  $b++; } echo $a . " " . $b; ?> 

    输出：1  6

 ps：短路运算符，$b = 7没有运行，$a强制转换为bool,如果$a被赋值为false，为0


#####2.

a.php:
	<?php echo '123'; ?>

b.php:

    <?php echo include('a.php');?>

运行b.php输出：

	1231

 
#####3.

	<?php
	    function timesTwo(&$int) {
	        $int = $int * 2;
	    }
	    $int = 2;
	    $result = timesTwo($int);
		echo $int;

*PS:*   &符号属于传引用，$int的值会传进去变化，不用return，$int值也能变，输出$int 为4


#####4.

	<?php
	    $a = '你猜我是谁';
	    if($a == 0)
		{
	    	echo "你是猪";
		}else{
		    echo "你是神";
		}



#####5.
	
	
	<?php
	    print (int)pow(2,69);

pow(x,y)函数输出x的y次方，结果输出 `0`


#####6.

`call_user_func`：php反射函数
	<?php
		error_reporting(E_ALL);
		function increment(&$var)
		{
		    $var++;
		}
		
		$a = 0;
		call_user_func('increment', $a);
		echo $a."\n";
		
		call_user_func_array('increment', array(&$a)); // You can use this instead before PHP 5.3
		echo $a."\n";

以上例程会输出：
	
	0
	1

