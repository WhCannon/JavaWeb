# CSS简介
  - [css与html结合](#css与html结合)
  - [选择器](#选择器)
  
  
  
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


## 选择器
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
