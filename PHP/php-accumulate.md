## 后端知识积累


>Author: 凤歌  
>本文地址： <a id="selfUrl" href=""><script type="text/javascript">document.write(window.location.href);
document.querySelector('#selfUrl').setAttribute('href', window.location.href)
</script></a>  
> 本文所述皆为作者整合互联网资料所得，不保证其准确性，因此会不定期修改  
> 为方便读者追本朔源， 请转载者保留此行。  
> 最近一直在研究前端， 所以对php疏松了一些，重新整理了一些笔记。发出来存为干货  


### 1.  原生PHP 中是不支持数字变量， 但是可以通过一个定位符实现
	
	<?php
		${1} = 1;
		echo ${1}; // 


### 2. php url 中有 xxxx/a.b/aaaa 这样的参数, 如何获取 a.b的值呢？

	$_GET("a.b") 是不行的
	$_GET("a_b") 可行


### 3. 是否为 三元运算中重复的写默认值恶心？ php > 5.3后， 可以省略掉第一个参数：

	$a = true;
	$a = $a?:false;


### 4. 如何处理 php 的闭包作用域问题：

php 5.3中可以使用匿名函数 function(){}.....

	$hehe = 1;
	$array = array(array(1), array(2));
	usort($array, function($a, $b){
		if ($hehe == 1)
			return $a > $b ? -1 : 1;

		return $a < $b ? -1 : 1;
	});

想通过一个变量去控制排序规则， 但是显然这样是不行的， 那么要怎么办呢？   
php 5.3 + 的版本中提供了一个方法 use () ， 可以把外部变量作用域赋予到匿名函数中。

	.....
	.....
	usort($array, function($a, $b) use ($hehe) {
		......
	});

	这样就可以了。


### 5. 秒数转化为时间：

	gmstrftime('%H:%M:%S',800);


### 6. 二维数组扁平化

    /**
     * 抽取多维数组的某个元素,组成一个新数组,使这个数组变成一个扁平数组
     * 使用方法:
     * <code>
     * <?php
     * $fruit = array(array('apple' => 2, 'banana' => 3), array('apple' => 10, 'banana' => 12));
     * $banana = Common::arrayFlatten($fruit, 'banana');
     * print_r($banana);
     * //outputs: array(0 => 3, 1 => 12);
     * ?>
     * </code>
     *
     * @access public
     * @param array $value 被处理的数组
     * @param string $key 需要抽取的键值
     * @return array
     */
    public static function array_flatten($value = array(), $key)
    {
        $result = array();

        if($value) 
        {
            foreach ($value as $inval) 
            {
                if(is_array($inval) && isset($inval[$key])) 
                {
                    $result[] = $inval[$key];
                } 
                else 
                {
                    break;
                }
            }
        }

        return $result;
    }


### 7. 使用 php5.3 + 的新特性实现多维数组扁平化
    
    $array = array( 1, 2, array( 3, 4, 5, array( 6, 7 ), 8, 9, ), 10, 11 );
    $res = array();
    array_walk_recursive($arr,function($v, $k) use (&$res){ $res[] = $v; });
    print_r($res);exit;

### 8. 不使用+ 号 ， 计算两个值的和

    function AddWithoutArithmetic($num1, $num2) {
        if(0 == $num2 )  return $num1;
        $sum = $num1 ^ $num2;
        $carry = ($num1 & $num2) << 1;
        return AddWithoutArithmetic($sum, $carry);
    }

    echo AddWithoutArithmetic(3, 5);

### 9. php 中多维数组中对汉字字典排序

	$arr = array(
	    array("你好"),
	    array("呵呵"),
	    array("不好"),
	    array("啊啊")
	);
	array_multisort($arr, SORT_STRING, SORT_DESC );
	
	print_r($arr);

### 10. PHP 批量删除数组中的元素

	//通过值删除数组中的元素 (2 & 3)

	$a = array(1, 2, 3, 4);
	$del = array(2,3);

	$res = array_diff($a, $del); //array(1, 4);

	需要注意一点的是 ， 这里用array_diff 是通过第一个参数作为参照， 第二个参数作为对比值来比较的，而且匹配 key
	如果需要匹配key的话， 使用 array_diff_assoc