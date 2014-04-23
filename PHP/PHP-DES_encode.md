PHP->JAVA DES 加密对接
--

PS：首先是起因，因为公司需要，我这边负责和其他公司的JAVA技术员对接程序，他们是提供接口方，是爷，所以算法由他们制定，采用的是 DES 对称加密，ECB模式 PKCS5-padding 填充模式。


其次点明两点：

1. DES 加密算法并非 PHP 内置加密函数，需要安装 mcrypt 扩展 （如果有能力手写算法也行）
2. mcrypt扩展需要手动安装，并非内置安装未启动模块

[Mcrypt 下载地址](http://sourceforge.net/projects/mcrypt/ "Mcrypt 下载地址")


[DES加密两种模式介绍](http://www.blogjava.net/wayne/archive/2011/05/23/350879.html)


在和Java的对接中出现各种问题，因为DES算法变数太多，任意一方有那么一点不同就对接不成功，这里仅仅是作为参考，不一定适合所有的des 加密对接。


来看一下代码：

	/**
	 * [mencrypt 获取Des加密结果]
	 * @param  [string] $encrypt [加密字符串]
	 * @param  [string] $key     [加密key]
	 * @return [string]          [加密结果]
	 */
	private function mencrypt($encrypt,$key="") {
		$encrypt = $this->pkcs5_pad($encrypt);

    $iv = mcrypt_create_iv ( mcrypt_get_iv_size ( MCRYPT_DES, MCRYPT_MODE_ECB ), MCRYPT_RAND );
    $passcrypt = mcrypt_encrypt ( MCRYPT_DES, $key, $encrypt, MCRYPT_MODE_ECB, $iv );
    $encode = base64_encode ( $passcrypt );
    //return bin2hex($encode);
    return $encode;
    }

这里使用的是 php 的mcrypt模块，`MCRYPT_MODE_ECB` 加密模式，加密后进行base64转码，另外，下面是调用的 pkcs5pad填充代码：

	private function pkcs5_pad($text)
	{
		$len = strlen($text);
		$mod = $len % 8;
		$pad = 8 - $mod;
		return $text.str_repeat(chr($pad),$pad);
	}



解密：

	/**
	 * [mencrypt 获取Des解密结果]
	 * @param  [string] $input [解密字符串]
	 * @param  [string] $key   [解密key]
	 * @return [string]        [解密结果]
	 */

  	private function mdecrypt($decrypt,$key="") {
    $decoded = base64_decode ( $decrypt );

    //$decoded = pack("H*", $decrypt);

    $iv = mcrypt_create_iv ( mcrypt_get_iv_size ( MCRYPT_DES, MCRYPT_MODE_ECB ), MCRYPT_RAND );
    $decrypted = mcrypt_decrypt ( MCRYPT_DES, $key, $decoded, MCRYPT_MODE_ECB, $iv );

    return $this->pkcs5_unpad($decrypted);
	}


上边解密、上上面加密代码中注释的两行代码是转格式的，2进制转16进制，逆转等。


反PKCS5-PADDING填充：

	private function pkcs5_unpad($text)
	{
		$pad = ord($text{strlen($text)-1});

		if ($pad > strlen($text)) return $text;
		if (strspn($text, chr($pad), strlen($text) - $pad) != $pad) return $text;
		return substr($text, 0, -1 * $pad);
	}



