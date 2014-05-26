php-curl-上传文件
--

>坑爹接口开发日记之三：curl上传文件的坑

>在那山的这边海的那边有一群程序猿，他们老实又胹腆，他们聪明但没钱。他们一天到晚坐在那里熬夜写软件，饿了就咬一口方便面。哦苦命的程序猿，哦苦命的程序猿，只要一改需求他们就要重新搞一遍，但是期限只剩下两天。


先看一下php-curl上传文件，怎么上传文件网上一大堆，我这里简单的提一下

curl自身提供的模拟file控件上传文件，前提是post数据必须是 array，比如：

	$postData = array(
		"param" => $urlData,
		"platId"=> $platId,
		"file"  => "@" . $file_url
	);

这样就可以上传图片了，乍一看没问题，就想官网说的一样，文件名使用的绝对路径，加了 @ 符号、使用了数组格式，而且我本地在windows环境下测试毫无问题，漫长的接口对接眼看就要结束了，突然之间发现了一个极其坑爹的问题（其实问题起因一直到现在还是猜测的，没有确定）


#### 凤歌德马赫猜想：

> php中通过curl上传文件的时候 windows下 contentType会自动识别还原，比如上传 iamge/jpeg , 上传成功后就还原成 image/jpeg 格式，而在unix环境下，上传成功后文件 contType 依然是二进制流格式 （application/octet-stream）

于是坑就出现了..无论怎么调也调不好，各种查阅资料查阅不到，网上相关资料甚少。

最终在官网发现了一个规则：

	"@file.jpg;type:image/jpeg" 

官网大法好！

于是我屁颠屁颠的去实践，结果爆了一个错误：

	Errnofailed creating formpost date
这个错误就是curl告诉我：你这个不是有效文件，少扯淡！

好吧，我继续去查资料，查了curl手册后发现 type:image/jpeg 这个规则只适用于 7.19+

好吧，赶紧去phpinfo() 一下，果不出其然，我是 7.16，换成7.19后，果然成功。

解决方案：

	$postData = array(
		"param" => $urlData,
		"platId"=> $platId,
		"file"  => "@" . $file_url . ";type=image/jpeg"
	);

