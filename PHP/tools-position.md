PHP实现判断一个点是否在一个区域内的算法
-

>PS：我不产生雨，我只是大自然的搬运工

>上海语镜.Primo.Chu

>2013/10/18 11:39:55 


#### 算法如下:


    $vertices_x = array(37.628134, 37.629867, 37.62324, 37.622424);    //假定一个区域-经度x范围
	$vertices_y = array(-77.458334,-77.449021,-77.445416,-77.457819); // 假定一个区域-经度y范围
	$points_polygon = count($vertices_x);  // number vertices = number of points in a self-closing polygon
	$longitude_x = $_GET["longitude"];  // x-coordinate of the point to test
	$latitude_y = $_GET["latitude"];    // y-coordinate of the point to test

	if (is_in_polygon($points_polygon, $vertices_x, $vertices_y, $longitude_x, $latitude_y)){
  		echo "在区域内!";
	}
	else echo "不在区域内！";


	function is_in_polygon($points_polygon, $vertices_x, $vertices_y, $longitude_x, $latitude_y)
	{
  		$i = $j = $c = $point = 0;
  		for ($i = 0, $j = $points_polygon ; $i < $points_polygon; $j = $i++) {
    $point = $i;
    if( $point == $points_polygon )
      $point = 0;
    if ( (($vertices_y[$point]  >  $latitude_y != ($vertices_y[$j] > $latitude_y)) &&
     ($longitude_x < ($vertices_x[$j] - $vertices_x[$point]) * ($latitude_y - $vertices_y[$point]) / ($vertices_y[$j] - $vertices_y[$point]) + $vertices_x[$point]) ) )
       $c = !$c;
  	}
 	 return $c;
	}



#### 计算两个点的距离: ####

	function getDistanceBetweenPointsNew($latitude1, $longitude1, $latitude2, $longitude2) {
    $theta = $longitude1 - $longitude2;
    $miles = (sin(deg2rad($latitude1)) * sin(deg2rad($latitude2))) + (cos(deg2rad($latitude1)) * cos(deg2rad($latitude2)) * cos(deg2rad($theta)));
    $miles = acos($miles);
    $miles = rad2deg($miles);
    $miles = $miles * 60 * 1.1515;
    $feet = $miles * 5280;
    $yards = $feet / 3;
    $kilometers = $miles * 1.609344;
    $meters = $kilometers * 1000;
    return compact('miles','feet','yards','kilometers','meters');
	}
	 
	$point1 = array('lat' => 40.770623, 'long' => -73.964367);
	$point2 = array('lat' => 40.758224, 'long' => -73.917404);
	$distance = getDistanceBetweenPointsNew($point1['lat'], $point1['long'], $point2['lat'], $point2['long']);
	foreach ($distance as $unit => $value) {
	    echo $unit.': '.number_format($value,4).'
	';
	}


#### 通过IP获得位置： ####

	function detect_city($ip) {
        
        $default = 'Hollywood, CA';

        if (!is_string($ip) || strlen($ip) < 1 || $ip == '127.0.0.1' || $ip == 'localhost')
            $ip = '8.8.8.8';

        $curlopt_useragent = 'Mozilla/5.0 (Windows; U; Windows NT 5.1; en-US; rv:1.9.2) Gecko/20100115 Firefox/3.6 (.NET CLR 3.5.30729)';
        
        $url = 'http://ipinfodb.com/ip_locator.php?ip=' . urlencode($ip);
        $ch = curl_init();
        
        $curl_opt = array(
            CURLOPT_FOLLOWLOCATION  => 1,
            CURLOPT_HEADER      => 0,
            CURLOPT_RETURNTRANSFER  => 1,
            CURLOPT_USERAGENT   => $curlopt_useragent,
            CURLOPT_URL       => $url,
            CURLOPT_TIMEOUT         => 1,
            CURLOPT_REFERER         => 'http://' . $_SERVER['HTTP_HOST'],
        );
        
        curl_setopt_array($ch, $curl_opt);
        
        $content = curl_exec($ch);
        
        if (!is_null($curl_info)) {
            $curl_info = curl_getinfo($ch);
        }
        
        curl_close($ch);
        
        if ( preg_match('{<li>City : ([^<]*)</li>}i', $content, $regs) )  {
            $city = $regs[1];
        }
        if ( preg_match('{<li>State/Province : ([^<]*)</li>}i', $content, $regs) )  {
            $state = $regs[1];
        }

        if( $city!='' && $state!='' ){
          $location = $city . ', ' . $state;
          return $location;
        }else{
          return $default; 
        }
        
    }


