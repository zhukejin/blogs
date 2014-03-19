登陆框抖动
-

	function shake(o){
	    var $panel = $("#"+o);
	    box_left = ($(window).width() -  $panel.width()) / 2;
	    $panel.css({'left': box_left,'position':'absolute'});
	    for(var i=1; 4>=i; i++){
	        $panel.animate({left:box_left-(40-10*i)},50);
	        $panel.animate({left:box_left+2*(40-10*i)},50);
	    }
	}


