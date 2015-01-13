通过 php 爬页面拿到页面上的title：
--
> 原创文章， 可能随时更改， 为了方便溯源，转载请保留此行，原文地址：[]()


    $ch = curl_init();
    curl_setopt($ch, CURLOPT_URL, "http://bbs.hupu.com/bxj");
    curl_setopt($ch, CURLOPT_HEADER, 0);
    curl_setopt($ch, CURLOPT_RETURNTRANSFER, 1);
    $data = curl_exec($ch);
    if(! mb_check_encoding($data, 'utf-8')) {
      $data = mb_convert_encoding($data,'UTF-8','gbk');
    }

    @eregi("<title>(.*)</title>", $data, $title);

    print_r($title[1]);exit;