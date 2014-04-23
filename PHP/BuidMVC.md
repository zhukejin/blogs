如何实现一个简单的MVC架构
--

>闲着无聊就写了个简单的MVC模型，粗陋之处多多包涵，第一次写漏洞百出，多多指教，以便改进和进步...



#### 先来看下基本的目录结构 ####

- core  -----------------------------核心文件夹
	- 404 ------------------------定义页面未找到
	- common ------------------用户自定义函数文件夹
	- config ---------------------配置文件夹
	- lib --------------------------核心抽象类文件夹
		- Controller.php -----控制器类
		- Model.php-----------模型类
		- view.php-------------视图类
- controller -----------------------用户控制器文件夹
- model----------------------------用户模型文件夹
- view------------------------------用户视图文件夹
- static-----------------------------静态文件文件夹
	- css
	- js
	- image
	- font

那么如何去实现`MVC`呢？

#### 1 . 首先你要有一个主入口文件 ...（这不是废话吗..）


那么我们在根目录建一个文件 `index.php`，这个文件的作用就是定义一个基本路径文件，所有操作都必须从这里进入。

那么如何让用户只能通过 `index.php` 访问整个网站呢？

这里我用了 `ACCESS_TOKEN` 的方法，就是在`inde.php` 中定义一个常量 `ACCESS_TOKEN` ，然后在其他页面都加上一个判断： 

	defined("ACCESS_TOKEN") or exit("您没有权限查看此文件");

这样就可以判断权限了..

这样主入口的文件全部内容就是：

	<?php
	define('ACCESS_TOKEN', '03d4d48ba01cecc702802ebb0a1536a01f736148');
	require "./core/route.php";
	require "./core/loader.php";
	exit;
	/*End Index.php*/

这里引用了两个文件 `route.php` 和 `loader.php`，分别用于控制 URL 路由和 引用文件的魔术加载。

route.php：

	<?php defined("ACCESS_TOKEN") or exit("您没有权限查看此文件");
	/**
	 * Route解析
	 */
	//获取用户输入的URL中全部参数
	$PARENTALL = $_SERVER['QUERY_STRING'];

	//如果用户没有输入完整的参数，则赋予默认参数 welceome/index
	$PARENTALL = !strrpos($PARENTALL,"/") ? "welcome/inedex" : $PARENTALL;
	
	//分割参数 （控制器/方法）
	$PARENT = explode("/", $PARENTALL);
	
	//这里强制用户控制器文件名和类名相同，以便查找
	$CNAME=$PARENT[0].'Controller';
	
	//判断如果没有此文件，容错提醒
	is_file('controller/'.$CNAME.'.php') or exit("没找到此控制器");

	//定义控制器文件引入路径
	$CPATH='controller/'.$CNAME.'.php';

	//获取方法名
	$METHOD=$PARENT[1];
	require($CPATH);
	$controller=new $CNAME; 

	//判断是否存在此方法，如果不存在报错返回
	method_exists($controller,$METHOD) or exit("未定义的方法");

	//调用控制器方法
	$controller->$METHOD();


