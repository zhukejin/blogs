## Q：API为什么加密？
 A：就安全来说，所有客户端和服务器端的通信内容应该都要通过加密通道(HTTPS)传输，明文的HTTP通道将会是man-in-the- middle及其各种变种攻击的温床。所谓man-in-the-middle攻击简单讲就是指恶意的黑客可以在客户端和服务器端的明文通信通道上做手 脚，黑客可以监听通信内容，偷取机密信息，甚至可以篡改通信内容，而通过加密后的通信内容理论上是无法被破译的。（我们目前仅仅是把加密作为一种验证手段，而不是把加密做加密信息），下文会详解。 


<!--more-->


## Q：我们的加密方法是什么？
A：目前我们的加密方法是利用agent和secret以及参数计算出签名sign来验证身份的。   

### 加密方法详解（以PHP请求得到用户资料为例子）
 第一步： 

	$array = array(
    'mobile'     =>     '12345678978',         //此号码不幸再次中枪 
	'agent'      =>     'abcdefg',               //一个movement标识码 
	'secret'     =>     'BB07FFE0317215532C8944A04CB053B27C3DD887' //对应的密文 );

// 对加密数组进行字典排序

	foreach ($array as $key=>$value){
	 $arr[$key] = $key; }
	 sort($arr); //字典排序的作用就是防止因为参数顺序不一致而导致下面拼接加密不同

// 将Key和Value拼接

	$str = "";
	foreach ($arr as $k => $v) {
	 $str = $str.$arr[$k].$array[$v];
	}

//通过sha1加密并转化为大写

	$sign = strtoupper(sha1($str));
//计算出sign压入数组，释放secret

	$array['sign'] = $sign;
	unset($array['secret']);
/* HTTP请求API */

	$apiUrl = 'url:apiurl';
	$co_1 = HttpClient :: quickPost($apiUrl,$array);
	print_r($co_1); 

##### OK，现在已经完事了，就这么简单，一步搞定     

## 然后呢….我们来说下签名机制的作用：   

**当第三方客户端访问我们的开放平台服务端，服务端需要先进行客户端身份进行认证，验证的目的在于两点——>**   
#### **1. 客户端合法性。**

** 客户端用密钥参与签名，但不作为请求参数传递。API服务器将根据客户端提交的`agent`，提取对应在服务端存储的密钥（`secret`）参与规则性验证(这里是指签名方法验证)，如果一致，则证实该请求客户端合法。** 
#### **2. 请求接口参数的完整性。**

**客户端的请求参数均参加公开签名方法，生成最后的签名参数值。签名参数将和其余参数一并参与HTTP请求。由于我们的请求时是用 http协议,可能发生网络劫持，部分参数可能会被篡改，为了保证请求安全，任何参数的修改或是缺失都会使得签名验证不正确，而被服务端拒绝访问。**   作为客户端，需要保留好官方颁发的`agent` & `secret` ，每个在官方注册的独立应用均有唯一的一对值，`agent`会在请求中作为一个应用标识参与接口请求的参数传递，`secret`将作为唯一不需要参数传递，但是它将作为验证当前请求的关键参数，只有应用开发者和颁发的服务端才知道。由于签名是依靠同样的算法加密实现，因此，应用端和服务端可以计算出相同的签名值，签名实际意义在于服务端对客户端的访问身份认证。在某种意义上签名机制有点类似用公钥方法签名，用每个应用对应的私钥值来解密，只是这种解密过程实质就是核对签名参数值的过程。 需要注意的是，一旦应用的私钥(`secrect`)发生泄漏，那么应用需要重新向开放平台请求修改密钥或是重新颁发新的 `agent` & `secret` 。否则，任何其他第三方盗用该应用的`agent`  & `secret`导致的请求都会视为合法。因为 `agent`  & `secret` 是每个应用的唯一身份标识。