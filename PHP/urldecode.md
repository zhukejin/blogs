不完整 url decode 解析方案：
--

	if (strlen($v['url']) > 200) {
        $v['url'] = rawurldecode($v['url']);
        $d = strrpos($v['url'], "=");
        $p = strrpos($v['url'], "%");

        if (($p + 1) == strlen($v['url']))
            $v['url'] = rtrim($v['url'], "%");
        
        if ($d > 0) {
            $param = substr($v['url'], $d+2);
            $s = explode("%", $param);
            if (strlen(end($s)) != 2) 
                array_pop($s);
            
            $l = count($s) % 3;
            for ($i=0; $i < $l; $i++) { 
                array_pop($s);
            }

            $afterparam = implode("%", $s);
            $beforeparam = substr($v['url'], 0, $d+2);
            $url = $beforeparam . $afterparam . "...";       
        } else {
            $url = $v['url'];
        }
    } else {
        $url = $v['url'];
    }


### js 实现方法：

	if (value.length > 199) {
        var arr = value.split('%');
        var d = arr.length % 3;
        if (d > 0) {
            for (var i = 0; i < d + 1; i++) {  //因为 arr[0] 为第一个%前的数据，不为空，则为不用解析的源字符
                arr.pop();
            };
        }
        value = arr.join("%");