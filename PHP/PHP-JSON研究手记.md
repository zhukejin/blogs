PHP-JSON 研究编码问题
-

	2013/06/18 11:45:39  
	Primo.Chu
	上海语镜汽车信息技术公司




正文
-
<br>
如同我们用到的，在 php 中使用 json_encode() 内置函数(php > 5.2)可以使用得 php 中数据可以与其它语言很好的传递并且使用它。

这个函数的功能是将数值转换成json数据存储格式。

下面这些GB2312代码：

    <?php
		$array = array(
			'name'	=>	'赵先生'
		);
		
		$jsoncode = json_encode($array);
		echo $jsoncode;


这个程序的执行结果是：

	{"name":null}

 json_encode 函数中中文被编码成 null 了，Google 了一下，很简单，为了与前端紧密结合，Json 只支持 utf-8 编码。

 接着看下面的代码：

	 <?php
		$array = array(
			'title'=>iconv('gb2312','utf-8','这里是中文标题')
		);
		
		$jsoncode = json_encode($array);
		echo $jsoncode;

这个程序的执行结果是：
	
	{"title":"\u8fd9\u91cc\u662f\u4e2d\u6587\u6807\u9898"} 

数组中所有中文在json_encode之后都不见了或者出现\u2353等。
解决方法是用urlencode()函数处理以下，在json_encode之前，把所有数组内所有内容都用urlencode()处理一下，然用json_encode()转换成json字符串，最后再用urldecode()将编码过的中文转回来。


### 以下示例代码： ###
	<?php 
	/**
	 *使用特定function对数组中所有元素做处理
	 *@param  string  &$array     要处理的字符串
	 *@param  string  $function   要执行的函数
	 *@return boolean $apply_to_keys_also     是否也应用到key上
	 *@access public
	 */ 

	function arrayRecursive(&$array, $function, $apply_to_keys_also = false) 
	{ 
	    static $recursive_counter = 0; 
	    if (++$recursive_counter > 1000) { 
	        die('possible deep recursion attack'); 
	    } 
	    foreach ($array as $key => $value) { 
	        if (is_array($value)) { 
	            arrayRecursive($array[$key], $function, $apply_to_keys_also); 
	        } else { 
	            $array[$key] = $function($value); 
	        } 
	  
	        if ($apply_to_keys_also && is_string($key)) { 
	            $new_key = $function($key); 
	            if ($new_key != $key) { 
	                $array[$new_key] = $array[$key]; 
	                unset($array[$key]); 
	            } 
	        } 
	    } 
	    $recursive_counter--; 
	} 
	  
	/*
	 * 将数组转换为JSON字符串（兼容中文）
	 * @param  array   $array      要转换的数组
	 * @return string      转换得到的json字符串
	 * @access public
	 *
	 */ 
	function JSON($array) { 
	    arrayRecursive($array, 'urlencode', true); 
	    $json = json_encode($array); 
	    return urldecode($json); 
	} 
	 
	$array = array("毛主席");
	echo JSON($array); 

这回可以了....
	
	
	
		