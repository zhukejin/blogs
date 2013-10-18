检测浏览器语言、多维数组排序
-

#### 检测浏览器语言，只提供可用的$availableLanguages作为数组(‘en’, ‘de’, ‘es’)

	function get_client_language($availableLanguages, $default='en'){
	if (isset($_SERVER['HTTP_ACCEPT_LANGUAGE'])) {
	$langs=explode(',',$_SERVER['HTTP_ACCEPT_LANGUAGE']);
	
	//start going through each one
	foreach ($langs as $value){
	$choice=substr($value,0,2);
	if(in_array($choice, $availableLanguages)){
	return $choice;
	}
	}
	} 
	return $default;
	}

####多维数组排序

	foreach($array as $value)
	{
	$contribution[] = $value['contribution'];
	}
	array_multisort($contribution,SORT_NUMERIC,SORT_DESC,$array);
	print_r($array);


