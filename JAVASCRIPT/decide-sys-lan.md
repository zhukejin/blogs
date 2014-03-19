系统+浏览器判断
-
	
	<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
	<html xmlns="http://www.w3.org/1999/xhtml">
	<head>
	<meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
	<title>js浏览器+系统判断</title>
	</head>
	
	<body>
	<div id="browser"></div>
	<script type="text/javascript">
	var _detectEnv = (function(){
	var ua = navigator.userAgent.toLowerCase();
	var browserList = {
	'Internet Explorer': !/opera/.test(ua) && /msie/.test(ua),
	'Maxthon': (window.external && (typeof (window.external.max_version) == "string")),
	'360': !/opera/.test(ua) && /msie/.test(ua) && /360/.test(ua) ,
	'Internet Explorer 6': !/opera/.test(ua) && /msie 6/.test(ua),
	'Internet Explorer 7': !/opera/.test(ua) && /msie 7/.test(ua),
	'Internet Explorer 8': !/opera/.test(ua) && /msie 8/.test(ua),
	'Internet Explorer 9': !/opera/.test(ua) && /msie 9/.test(ua),
	'Internet Explorer 10': !/opera/.test(ua) && /msie 10/.test(ua),
	'Firefox': !(/compatible|webkit|khtml/).test(ua) && /firefox/.test(ua),
	'Opera': /opera/.test(ua),
	'Chrome': /chrome/.test(ua),
	'Safari': !/chrome/.test(ua) && (/webkit|khtml/).test(ua)
	},platformList = {
	'Mac OS X': (/macintosh|mac os x/).test(ua),
	'Linux': /linux/.test(ua),
	'Windows': (/windows|win32/).test(ua),
	'Window2000': (/windows nt 5.0|windows 2000/).test(ua),
	'WindowXP': (/windows nt 5.1|windows xp/).test(ua),
	'WindowVista': (/window nt 6.0|windows vista/).test(ua),
	'Window7': (/windows nt 6.1|windows 7/).test(ua)
	};
	return {
	getEnv: function(){
	var browser, platform;
	for(var b in browserList){
	if(browserList[b]){
	browser = b;
	}
	}
	for(var p in platformList){
	if(platformList[p]){
	platform = p;
	}
	}
	return browser + ' for ' + platform;
	}
	}
	})();
	document.getElementById('browser').innerHTML = _detectEnv.getEnv();
	</script>
	</body>
	</html>

