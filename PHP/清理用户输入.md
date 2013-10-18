清理用户输入
-

比较懒..不想解释..很好用的正则规则
	
	<?php
	function cleanInput($input) {
	
	  $search = array(
	    '@<script[^>]*?>.*?</script>@si',   // Strip out javascript
	    '@<[\/\!]*?[^<>]*?>@si',            // Strip out HTML tags
	    '@<style[^>]*?>.*?</style>@siU',    // Strip style tags properly
	    '@<![\s\S]*?--[ \t\n\r]*>@'         // Strip multi-line comments
	  );
	
	    $output = preg_replace($search, '', $input);
	    return $output;
	  }

	function sanitize($input) {
	    if (is_array($input)) {
	        foreach($input as $var=>$val) {
	            $output[$var] = sanitize($val);
	        }
	    }
	    else {
	        if (get_magic_quotes_gpc()) {
	            $input = stripslashes($input);
	        }
	        $input  = cleanInput($input);
	        $output = mysql_real_escape_string($input);
	    }
	    return $output;
	}
