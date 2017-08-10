Sass 笔记
--

> 很久没有写CSS，最近写Css 的时候发现很多 Sass 语法忘记了，本着好记性不如烂笔头的原则，开一篇记录一下常用的 Sass 语法
> 为方便读者追本索源，转载请保留本文地址

### 变量

变量使用 $ 来声明，ex:

```
$smallFont: 12px;
$normalFont: 14px;
$largeFont: 16px;

$baseColor: #E86295;
```

使用的时候:

```
color: $baseColor;
font-size: $smallFont;
```

假如变量需要拼接，则需要使用 `#{$baseColor}` 这样的语法来拼接，如：

```
$calc: 306 + 32;
height: calc(100vh - #{$calc});
```
### 方法

方法、关键字指令使用 @ 来标记， ex:

```
@function mySize($size) {
	@return $size + 24 + px;
}
```

使用的时候直接

```
font-size: mySize(12);
```
### 循环

使用 `each` 关键字:

```
@each $type in center, left, right {
  .text-#{$type} {
    text-align: $type;
  }
}
```

### 逻辑判断

使用 `if` 关键字:

```
@function mySize($size) {
	@if $size < 14 {
		@return $size + px;	
	} @else {
		@return $size + 24 + px;		
	}
}
``` 

### 混淆

混淆的作用是分离代码块, 在 scss 中导入 混淆即可引入代码块

```
@mixin button() {
	font-size: 14px;
	text-align: center;
	padding: 10px 20px;
}
```
使用的时候：

```
.my-btn {
	...
	@include button();
	...
}
```






