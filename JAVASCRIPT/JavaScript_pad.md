Javascript- array_Pad
--
> 在 php 中有 Array_pad 方法，这是一个很实用的方法，将数组填充到指定的长度。  

例如 

	array_pad(array(1,2), 4, 0)

结果： 

	=> array(1,2,0,0).
	
用元素`0` 将数组填充到长度4.  

在JavaScript中是没有这样封装好的方法，所以需要每次去循环push，显然很繁琐，这里封装一个 array_pad 方法：
	
	function array_pad (input, padSize, padValue) {
	var pad = []
	var newArray = []
	var newLength
	var diff = 0
	var i = 0

	if (Object.prototype.toString.call(input) === '[object Array]' && !isNaN(padSize)) {
		newLength = ((padSize < 0) ? (padSize * -1) : padSize)
		diff = newLength - input.length

		if (diff > 0) {
			for (i = 0; i < diff; i++) {
				newArray[i] = padValue
			}
			pad = ((padSize < 0) ? newArray.concat(input) : input.concat(newArray))
		} else {
			pad = input
		}
	}

	return pad
	}