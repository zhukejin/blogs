#你或许不需要jquery


>原文地址：http://youmightnotneedjquery.com/
>
>由 @zackbloom and @adamfschwartz 发布在 HubSpot.

>本来想翻译来着，但是太长了，加上本人英语也是战五渣，所以就英文直接拿过来了，想必懂一些JQ API的都能看懂...



<center><h2>AJAX</h2></center>

<center><h3>json</h3></center>

JQUERY:

	$.getJSON('/my/url', function(data) {
	
	});

IE9+:

	request = new XMLHttpRequest();
	request.open('GET', '/my/url', true);
	
	request.onload = function() {
	  if (request.status >= 200 && request.status < 400){
	    // Success!
	    data = JSON.parse(request.responseText);
	  } else {
	    // We reached our target server, but it returned an error
	
	  }
	};
	
	request.onerror = function() {
	  // There was a connection error of some sort
	};
	
	request.send();


<center><h3>Request</h3></center>

JQUERY:

	$.ajax({
	  type: 'GET',
	  url: '/my/url',
	  success: function(resp) {
	
	  },
	  error: function() {
	
	  }
	});


IE9+:

	request = new XMLHttpRequest();
	request.open('GET', '/my/url', true);
	
	request.onload = function() {
	  if (request.status >= 200 && request.status < 400){
	    // Success!
	    resp = request.responseText;
	  } else {
	    // We reached our target server, but it returned an error
	
	  }
	};
	
	request.onerror = function() {
	  // There was a connection error of some sort
	};
	
	request.send();


<center><h3>Post</h3></center>

JQUERY:

	$.ajax({
	  type: 'POST',
	  url: '/my/url',
	  data: data
	});

IE8+:

	var request = new XMLHttpRequest();
	request.open('POST', '/my/url', true);
	request.setRequestHeader('Content-Type', 'application/x-www-form-urlencoded; charset=UTF-8');
	request.send(data);


<center><h2>EFFECTS</h2></center>

<center><h3>Fade In</h3></center>

JQUERY:

	$(el).fadeIn();

IE9+:

	function fadeIn(el) {
	  el.style.opacity = 0;
	
	  var last = +new Date();
	  var tick = function() {
	    el.style.opacity = +el.style.opacity + (new Date() - last) / 400;
	    last = +new Date();
	
	    if (+el.style.opacity < 1) {
	      (window.requestAnimationFrame && requestAnimationFrame(tick)) || setTimeout(tick, 16)
	    }
	  };
	
	  tick();
	}
	
	fadeIn(el);

<center><h3>Hide</h3></center>

JQUERY:

	$(el).hide();

IE8+:

	el.style.display = 'none';

<center><h3>Show</h3></center>

JQUERY:

	$(el).show();

IE8+:

	el.style.display = '';


<center><h2>ELEMENTS</h2></center>


<center><h3>Add Class</h3></center>

JQUERY:

	$(el).addClass(className);

IE8+:

	if (el.classList)
	  el.classList.add(className);
	else
	  el.className += ' ' + className;

<center><h3>After</h3></center>

JQUERY:

	$(el).after(htmlString);

IE8+:

	el.insertAdjacentHTML('afterend', htmlString);


<center><h3>Append</h3></center>

JQUERY:

	$(parent).append(el);

IE8+:

	parent.appendChild(el);

<center><h3>Before</h3></center>

JQUERY:

	$(el).before(htmlString);

IE8+:

	el.insertAdjacentHTML('beforebegin', htmlString);

<center><h3>Children</h3></center>

JQUERY:

	$(el).children();

IE9+:

	el.children

<center><h3>Clone</h3></center>

JQUERY:

	$(el).clone();

IE8+:

	el.cloneNode(true);

<center><h3>Contains</h3></center>

JQUERY:

	$.contains(el, child);

IE8+:

	el !== child && el.contains(child);

<center><h3>Contains Selector</h3></center>

JQUERY:

	$(el).find(selector).length;

IE8+:

	el.querySelector(selector) !== null


<center><h3>Each</h3></center>

JQUERY:

	$(selector).each(function(i, el){
	
	});

IE9+:

	var elements = document.querySelectorAll(selector);
	Array.prototype.forEach.call(elements, function(el, i){
	
	});

<center><h3>Empty</h3></center>

JQUERY:

	$(el).empty();

IE9+:

	el.innerHTML = '';

<center><h3>Filter</h3></center>

JQUERY:

	$(selector).filter(filterFn);

IE9+:

	Array.prototype.filter.call(document.querySelectorAll(selector), filterFn);

<center><h3>Finding Children</h3></center>

JQUERY:

	$(el).find(selector);

IE8+:

	el.querySelectorAll(selector);

<center><h3>Finding Elements</h3></center>

JQUERY:

	$('.my #awesome selector');

IE8+:

	document.querySelectorAll('.my #awesome selector');

<center><h3>Get Style</h3></center>

JQUERY:

	$(el).css(ruleName);

IE9+:

	getComputedStyle(el)[ruleName];

<center><h3>Getting Attributes</h3></center>

JQUERY:

	$(el).attr('tabindex');

IE8+:

	el.getAttribute('tabindex');


<center><h3>Getting Html</h3></center>

JQUERY:

	$(el).html();

IE8+:

	el.innerHTML

<center><h3>Getting Outer Html</h3></center>

JQUERY:

	$('<div>').append($(el).clone()).html();

IE8+:

	el.outerHTML

<center><h3>Getting Text</h3></center>

JQUERY:

	$(el).text();

IE9+:

el.textContent

<center><h3>Has Class</h3></center>

JQUERY:

	$(el).hasClass(className);

IE8+:

	if (el.classList)
	  el.classList.contains(className);
	else
	  new RegExp('(^| )' + className + '( |$)', 'gi').test(el.className);


<center><h3>Matches</h3></center>

JQUERY:

	$(el).is($(otherEl));

IE8+:

	el === otherEl

<center><h3>Matches Selector</h3></center>

JQUERY:

	$(el).is('.my-class');

IE9+:

	var matches = function(el, selector) {
	  return (el.matches || el.matchesSelector || el.msMatchesSelector || el.mozMatchesSelector || el.webkitMatchesSelector || el.oMatchesSelector).call(el, selector);
	};
	
	matches(el, '.my-class');


<center><h3>Next</h3></center>

JQUERY:

	$(el).next();

IE9+:

	el.nextElementSibling


<center><h3>Offset</h3></center>

JQUERY:

	$(el).offset();

IE8+:

	var rect = el.getBoundingClientRect()
	
	{
	  top: rect.top + document.body.scrollTop,
	  left: rect.left + document.body.scrollLeft
	}

<center><h3>Offset Parent</h3></center>

JQUERY:

	$(el).offsetParent();

IE8+:

	el.offsetParent || el

<center><h3>Outer Height</h3></center>

JQUERY:

	$(el).outerHeight();

IE8+:

	el.offsetHeight


<center><h3>Outer Height With Margin</h3></center>

JQUERY:

	$(el).outerHeight(true);

IE9+:

	function outerHeight(el){
	  var height = el.offsetHeight;
	  var style = getComputedStyle(el);
	
	  height += parseInt(style.marginTop) + parseInt(style.marginBottom);
	  return height;
	}
	
	outerHeight(el);


<center><h3>Outer Width</h3></center>

JQUERY:

	$(el).outerWidth();

IE8+:

	el.offsetWidth


<center><h3>Outer Width With Margin</h3></center>

JQUERY:

	$(el).outerWidth(true);

IE9+:

	function outerWidth(el){
	  var height = el.offsetWidth;
	  var style = getComputedStyle(el);
	
	  height += parseInt(style.marginLeft) + parseInt(style.marginRight);
	  return height;
	}
	
	outerWidth(el);


<center><h3>Parent</h3></center>

JQUERY:

	$(el).parent();

IE8+:

	el.parentNode


<center><h3>Position</h3></center>

JQUERY:

	$(el).position();

IE8+

	{left: el.offsetLeft, top: el.offsetTop}


<center><h3>Position Relative To Viewport</h3></center>

JQUERY

	var offset = el.offset();
	
	{
	  top: offset.top - document.body.scrollTop,
	  left: offset.left - document.body.scrollLeft
	}

IE8+

	el.getBoundingClientRect()


<center><h3>Prepend</h3></center>

JQUERY

	$(parent).prepend(el);
IE8+
	
	parent.insertBefore(el, parent.firstChild);


<center><h3>Prev</h3></center>

JQUERY:

	$(el).prev();

IE9+

	el.previousElementSibling


<center><h3>Remove</h3></center>

JQUERY

	$(el).remove();
IE8+

	el.parentNode.removeChild(el);


<center><h3>Remove Class</h3></center>

JQUERY

	$(el).removeClass(className);
IE8+

	if (el.classList)
	  el.classList.remove(className);
	else
	  el.className = el.className.replace(new RegExp('(^|\\b)' + className.split(' ').join('|') + '(\\b|$)', 'gi'), ' ');

<center><h3>Replacing From Html</h3></center>

JQUERY

	$(el).replaceWith(string);
IE8+

	el.outerHTML = string;

<center><h3>Set Style</h3></center>

JQUERY

	$(el).css('border-width', '20px');
IE8+

// Use a class if possible

	el.style.borderWidth = '20px';

<center><h3>Setting Attributes</h3></center>

JQUERY

	$(el).attr('tabindex', 3);
IE8+

	el.setAttribute('tabindex', 3);

<center><h3>Setting Html</h3></center>

JQUERY

	$(el).html(string);
IE8+

	el.innerHTML = string;

<center><h3>Setting Text</h3></center>

JQUERY

	$(el).text(string);
IE9+

	el.textContent = string;

<center><h3>Siblings</h3></center>

JQUERY

	$(el).siblings();
IE9+

	Array.prototype.filter.call(el.parentNode.children, function(child){
	  return child !== el;
	});

<center><h3>Toggle Class</h3></center>

JQUERY

	$(el).toggleClass(className);

IE9+

	if (el.classList) {
	  el.classList.toggle(className);
	} else {
	  var classes = el.className.split(' ');
	  var existingIndex = classes.indexOf(className);
	
	  if (existingIndex >= 0)
	    classes.splice(existingIndex, 1);
	  else
	    classes.push(className);
	
	  el.className = classes.join(' ');
	}



<center><h2>EVENTS</h2></center>


<center><h3>Off</h3></center>

JQUERY

	$(el).off(eventName, eventHandler);
IE9+

	el.removeEventListener(eventName, eventHandler);

<center><h3>On</h3></center>

JQUERY

	$(el).on(eventName, eventHandler);
IE9+

	el.addEventListener(eventName, eventHandler);


<center><h3>Ready</h3></center>

JQUERY

	$(document).ready(function(){
	
	});

IE9+

	document.addEventListener('DOMContentLoaded', function(){
	
	});

<center><h3>Trigger Custom</h3></center>

JQUERY

	$(el).trigger('my-event', {some: 'data'});

IE9+

	if (window.CustomEvent) {
	  var event = new CustomEvent('my-event', {detail: {some: 'data'}});
	} else {
	  var event = document.createEvent('CustomEvent');
	  event.initCustomEvent('my-event', true, true, {some: 'data'});
	}
	
	el.dispatchEvent(event);

<center><h3>Trigger Native</h3></center>

JQUERY

	$(el).trigger('change');

IE9+

// For a full list of event types: [https://developer.mozilla.org/en-US/docs/Web/API/document.createEvent](https://developer.mozilla.org/en-US/docs/Web/API/document.createEvent "点这里")

	event = document.createEvent('HTMLEvents');
	event.initEvent('change', true, false);
	el.dispatchEvent(event);

<center><h2>UTILS</h2></center>

<center><h3>Array Each</h3></center>

JQUERY

	$.each(array, function(i, item){
	
	});

IE9+

	array.forEach(function(item, i){
	
	});


<center><h3>Bind</h3></center>

JQUERY

	$.proxy(fn, context);

IE9+

	fn.bind(context);


<center><h3>Deep Extend</h3></center>

JQUERY

	$.extend(true, {}, objA, objB);

IE8+

	var deepExtend = function(out) {
	  out = out || {};
	
	  for (var i = 1; i < arguments.length; i++) {
	    var obj = arguments[i];
	
	    if (!obj)
	      continue;
	
	    for (var key in obj) {
	      if (obj.hasOwnProperty(key)) {
	        if (typeof obj[key] === 'object')
	          deepExtend(out[key], obj[key]);
	        else
	          out[key] = obj[key];
	      }
	    }
	  }
	
	  return out;
	};

	deepExtend({}, objA, objB);


<center><h3>Extend</h3></center>

JQUERY

	$.extend({}, objA, objB);

IE8+

	var extend = function(out) {
	  out = out || {};
	
	  for (var i = 1; i < arguments.length; i++) {
	    if (!arguments[i])
	      continue;
	
	    for (var key in arguments[i]) {
	      if (arguments[i].hasOwnProperty(key))
	        out[key] = arguments[i][key];
	    }
	  }
	
	  return out;
	};
	
	extend({}, objA, objB);


<center><h3>Index Of</h3></center>

JQUERY

	$.inArray(item, array);

IE9+
	
	array.indexOf(item);


<center><h3>Is Array</h3></center>

JQUERY

	$.isArray(arr);

IE9+

	Array.isArray(arr);

<center><h3>Map</h3></center>


JQUERY

	$.map(array, function(value, index){
	
	});

IE9+

	array.map(function(value, index){
	
	});


<center><h3>Now</h3></center>

JQUERY

	$.now();

IE9+

	Date.now();

<center><h3>Parse Html</h3></center>

JQUERY

	$.parseHTML(htmlString);

IE9+

	var parseHTML = function(str) {
	  var tmp = document.implementation.createHTMLDocument();
	  tmp.body.innerHTML = str;
	  return tmp.body.children;
	};
	
	parseHTML(htmlString);


<center><h3>Parse Json</h3></center>

JQUERY

	$.parseJSON(string);

IE8+

	JSON.parse(string);

<center><h3>Trim</h3></center>

JQUERY

	$.trim(string);

IE9+

	string.trim();


<center><h3>Type</h3></center>

JQUERY

	$.type(obj);

IE8+

	Object.prototype.toString.call(obj).replace(/^\[object (.+)\]$/, "$1").toLowerCase();
