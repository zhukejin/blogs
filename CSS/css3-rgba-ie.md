CSS3 rgba() 兼容IE
--

> 本文系作者整合互联网资料加个人理解所写，或随时更改， 若需转载请保留此行，以便后来者追本朔源，本文地址：

所谓不登高山， 不知山之高也。之前无过多接触CSS， 固步自封之下也不觉得CSS多么强大， 后来逐渐了解了点CSS3，觉得CSS已有发展为一门独立语言的潜力。

临渊羡鱼，不如退而结网， 总是羡慕别人的网站特效多好， 还是自己来研究CSS3吧。

CSS3效果好归好， 但是总是能碰到一些恶心的兼容问题，比如当下的 rgba ... 

	.wrap {
        background: rgba(0,0,0,0.8);
        width: 100px;
        height: 100px;
    }

在 chrome ， firefox ， IE10 下开开心心的跑了起来， 但是在 ie8上是不会出现背景的，为什么？

rgba 做为css3 属性，例行不支持IE，如果想要解决这个问题， 需要调用 IE的 “滤镜”， filter属性。

	   .wrap {
        background: rgba(0,0,0,0.8);
        width: 100px;
        height: 100px;
        filter:progid:DXImageTransform.Microsoft.gradient(startColorstr=#BF000000,endColorstr=#BF000000); 
    }


#### 关于 progid:DXImageTransform.Microsoft.gradient 的属性：

1. enabled: 可选项。布尔值(Boolean)。设置或检索滤镜是否激活。 true | false
	1.  true: 默认值。滤镜激活。 
	2.  false:滤镜被禁止。
	
2. startColorStr:可选项。字符串(String)。设置或检索色彩渐变的开始颜色和透明度。 
	1. 其格式为 #AARRGGBB 。 AA 、 RR 、 GG 、 BB 为十六进制正整数。取值范围为 00 - FF 。 RR 指定红色值， GG 指定绿色值， BB 指定蓝色值，参阅 #RRGGBB 颜色单位。 AA 指定透明度。 00 是完全透明。 FF 是完全不透明。超出取值范围的值将被恢复为默认值。 
　　取值范围为 #FF000000 - #FFFFFFFF 。默认值为 #FF0000FF 。不透明蓝色。 
　　
3. EndColorStr:可选项。字符串(String)。设置或检索色彩渐变的结束颜色和透明度。参阅 startColorStr 属性。默认值为 #FF000000 。不透明黑色。 

#### 特性： 

1. Enabled:可读写。布尔值(Boolean)。参阅 enabled 属性。 
2. GradientType:可读写。整数值(Integer)。设置或检索色彩渐变的方向。1 | 0
	1. 1 = 默认值。水平渐变。
	2. 0 = 垂直渐变。  
　　
3. StartColorStr:可读写。字符串(String)。参阅 startColorStr 属性。 
	1. StartColor:可读写。整数值(Integer)。设置或检索色彩渐变的开始颜色。 取值范围为 0 - 4294967295 。
		1.  0 为透明。
		2.  4294967295 为不透明白色。 
4. EndColorStr:可读写。字符串(String)。设置或检索色彩渐变的结束颜色和透明度。参阅 startColorStr 属性。默认值为 #FF000000 。不透明黑色。 
5. EndColor:可读写。整数值(Integer)。设置或检索色彩渐变的结束颜色。 取值范围为 0 - 4294967295 。 0 为透明。 4294967295 为不透明白色。当在脚本中使用此特性时，也可以用十六进制格式： 0xAARRGGBB 。 

IE4.0以上支持的滤镜属性表 滤镜效果 描述 ：

- Alpha 设置透明度 
- Blru 建立模糊效果 
- Chroma 把指定的颜色设置为透明 
- DropShadow 建立一种偏移的影象轮廓，即投射阴影 
- FlipH 水平反转 
- FlipV 垂直反转 
- Glow 为对象的外边界增加光效 
- Grayscale 降低图片的彩色度 
- Invert 将色彩、饱和度以及亮度值完全反转建立底片效果 
- Light 在一个对象上进行灯光投影 
- Mask 为一个对象建立透明膜 
- Shadow 建立一个对象的固体轮廓，即阴影效果 
- Wave 在X轴和Y轴方向利用正弦波纹打乱图片 
- Xray 只显示对象的轮廓 

filter作为微软独有的一个属性， 不得不说，IE其实还是很强大的， 只不过走了非同寻常的道路， 没有被 w3c 规范起来而已..(*看起来跟党走，才是王道*)