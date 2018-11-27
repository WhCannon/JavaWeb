# CSS简介
  - [css与html结合](#css与html结合)
  - [基本选择器](#基本选择器)
  - [扩展选择器](#扩展选择器)
  - [盒子模型](#盒子模型)
  
  
## css与html结合
### 方式一

### 方式二
```html
<style type="text/css">
			css代码;
</style>

 <head>
  <style type="text/css">	
	div {
		background-color:green;
		color: red;
	}		
 </style>

 </head>
 <body>
<div>其意博</div>
 </body>
```


### 方式四
```html
<head>
  <title>HTML示例</title>
  <link rel="stylesheet" type="text/css" href="div.css" />
 </head>
```


## 基本选择器
### 标签选择器
```html
<html>
 <head>
  <title>HTML示例</title>
  <style type="text/css">

	div {
		background-color: red;
	}

	p {
		background-color: green;
	}
  </style>
 </head>
 <body>

<div>变化之由表，死生之兆彰，不谋而遗迹自同，1111</div>

<p>勿约而幽明斯契，稽其言有微，验之事不忒，诚可谓至道之宗，</p>

 </body>
</html>
```

### class选择器
```html
<html>
 <head>
  <title>HTML示例</title>
  <style type="text/css">

	/*div.haha {
		background-color: yellow;
	}

	p.haha {
		background-color: yellow;
	}*/

	.haha {
		background-color: orange;
	}


  </style>
 </head>
 <body>

<div class="haha">变化之由表，死生之兆彰，不谋而遗迹自同，1111</div>

<p class="haha">变化之由表aaaaaaaaaa</p>

 </body>
```

### id选择器
```html
<html>
 <head>
  <title>HTML示例</title>
  <style type="text/css">

	/*div#hehe {
		background-color: gray;
	}

	p#hehe {
		background-color: gray;
	}*/

	#hehe {
		background-color: #333300;
	}
  </style>
 </head>
 <body>

<div id="hehe">故动则有成，犹鬼神幽赞，而命世奇杰，时时间出焉</div>

<p id="hehe">aaaaaaaaaaaaaaaaaaaaaa</p>
 </body>
```

## 扩展选择器
### 伪类元素选择器
```html
<html>
 <head>
  <title>HTML示例</title>
  <style type="text/css">

	/*原始状态*/
	a:link {
		background-color: red;
	}

	/*悬停状态*/
	a:hover {
		background-color: green;
	}
	/*点击状态*/
	a:active {
		background-color: blue;
	}

	/*点击之后的状态*/
	a:visited {
		background-color: gray;
	}

  </style>
 </head>
 <body>

<a href="http://www.sina1.com.cn" target="_blank">CSS概述和与HTML的结合方式超链接一</a>

 </body>
</html>
```

## 盒子模型
###边框
```html
<html>
 <head>
  <title>HTML示例</title>
  <style type="text/css">

	div {
		width: 200px;
		height: 100px;

		border: 2px solid blue;
	}

	#div12 {
		border-right: 2px dashed yellow;
	}
  </style>
 </head>
 <body>

<div id="div11">AAAAAAAAAAAAAAA</div>
<div id="div12">BBBBBBBBBBBBBBB</div>
<div id="div13">CCCCCCCCCCCCCCC</div>
 </body>
</html>
```
