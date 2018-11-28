# JavaScript
  - [基本语法](#基本语法)
  - [String](#string)
  - [Array](#array)
  - [Date](#date)
  - [Math](#math)
  - [全局函数](#全局函数)
  - [函数重载问题](#函数重载问题)
  - [bom对象](#bom对象)
  - [dom对象](#dom对象)
  - [练习](#练-习)




## 基本语法
### 定义方法
```html
<script type="text/javascript">		
		function test() {
			alert("qqqqq");
		}
		//调用方法
		test();

		function add1(a,b) {
			var sum = a+b;
			alert(sum);		
		}
		add1(2,3);

		function add2(a,b,c) {
			var sum1 = a+b+c;
			return sum1;
		}
		alert(add2(3,4,5));		
	</script>

```

## String
```html
<script type="text/javascript">
		var str = "abcde";
		document.write(str.length);//长度

		var str1 = "asdf";
		document.write(str1.bold());//字符串加粗

		var str2 = "www";
		document.write(str2.fontcolor("red"));//设置字符串的颜色

		var str3 = "qqqqq";
		document.write(str3.fontsize(5));//设置字体大小

		var str4 = "haha";
		document.write(str4.link("hello.html"));//将字符串显示成超链接

		var s1 = "100";
		var s2 = "200";
		var s3 = "300"
		document.write(s1.sub());//下标
		document.write(s3);
		document.write(s2.sup());//上标
	</script>

<script type="text/javascript">
 
		var str1 = "abc";
		var str2 = "dfg";
		document.write(str1.concat(str2));//连接字符串

		var str3 = "abcdefg";
		document.write(str3.charAt(0)); //返回指定位置的字符
		document.write(str3.charAt(20)); //位置不存在，返回空字符串("")
		
		var str4 = "poiuyt";
		document.write(str4.indexOf("oiu"));//返回字符串首次出现的位置
		document.write(str4.indexOf("w")); //字符不存在，返回-1

		var str5 = "a-b-c-d";
		var arr1 = str5.split("-");//把字符串切割为数组
		document.write("length: "+arr1.length);
		
		var str6 = "abcd";
		document.write(str6.replace("a","Q"));//替换

		var str7 = "abcdefghuiop";
		document.write(str7.substr(5,5));  //fghui,从第五位开始，向后截取五个字符

		document.write("<br/>");
		document.write(str7.substring(3,5)); //de，截取[3,5)位置的字符
	</script>
```

## Array
```html
<script type="text/javascript">
		var arr1 = [1,2,3];
		document.write(arr1.length);//获取长度

		var arr11 = [1,2,3];
		var arr12 = [4,5,6];
		document.write(arr11.concat(arr12));//连接数组，并返回结果

		var arr13 = ["a","b","c"];
		document.write(arr13.join("-"));//根据指定字符把数组分隔(默认是逗号)

		var arr14 = ["a","b","c"];
		document.write("new length: "+arr14.push("zhangsan"));//在数组末尾添加元素，并返回新的长度

		var arr15 = ["aaa","bbb","ccc"];
		var arr16 = ["www","qqq"];
		document.write("new length:"+arr15.push(arr16));//length=4,把arr16整体当做一个字符串
		document.write("new array: "+arr15);

		var arr17 = ["zhangsan","lisi","wangwu","zhaoliu"];
		document.write("return: "+arr17.pop());//删除并返回最后一个元素
		document.write("new array: "+arr17);

		var arr18 = ["zhangsan1","lisi1","zhaoliu1","niuqi1"];
		document.write("new array:"+arr18.reverse());//颠倒数组中元素的顺序
		
	</script>
```
## Date
```html
<script type="text/javascript">
		var date = new Date();//获取当前时间
		document.write(date); // Fri Apr 17 10:47:46 UTC+0800 2015 
		document.write(date.toLocaleString());//转换成本地格式
		document.write("year: "+date.getFullYear());//获取当前年份
		document.write("month: "+(date.getMonth()+1));//获取月份
		document.write("week: "+date.getDay());//获取星期几(0代表星期日,6代表星期六)
		document.write("day: "+date.getDate());//获取月中的天数
		document.write("hour: "+date.getHours());//获取小时
		document.write("minute: "+date.getMinutes());//获取分钟
		document.write("second: "+date.getSeconds());//获取秒
		document.write("times: "+date.getTime());//获取毫秒
	</script>
```

## Math
```html
<script type="text/javascript">
		var mm = 10.7;
		document.write("ceil: "+Math.ceil(mm));//向上取整
		document.write("floor: "+Math.floor(mm));//向下取整	
		document.write("round: "+Math.round(mm));//四舍五入
		document.write(Math.random());//0.0-1.0的随机数
		document.write(Math.floor(Math.random()*10));//0-9的随机数
		document.write(Math.pow(2,5));//2的5次方
	</script>
```

## 全局函数
```html
<script type="text/javascript">	
		var str = "alert('1234');";
		alert(str);
		eval(str);//执行js代码（如果字符串是一个js代码，可以用eval直接执行）

		var str1 = "测试中文aaa1234";
		var encode1 = encodeURI(str1);//对字符编码
		document.write(encode1);
		var decode1 = decodeURI(encode1);//对字符解码
		document.write(decode1);

		var str2 = "aaaa";
		alert(isNaN(str2));//是否是数字，是数字返回false，不是返回true

		var str3 = "123";
		document.write(parseInt(str3)+1);//124,把字符串转换成数字
	</script>
```
## 函数重载问题
```html
<script type="text/javascript">	
		function add1() {
			if(arguments.length == 2) {
				return arguments[0]+arguments[1];
			} else if (arguments.length == 3) {
				return arguments[0]+arguments[1]+arguments[2];
			} else if (arguments.length == 4) {
				return arguments[0]+arguments[1]+arguments[2]+arguments[3];
			} else {
				return 0;
			}
		}
		alert(add1(1,2)); //3
		alert(add1(1,2,3)); //6
		alert(add1(1,2,3,4));  // 10
		alert(add1(1,2,3,4,5));  // 0
	</script>
```

## bom对象
### history
```html
<body>
	<input type="button" value="back" onclick="back1();"/>
	<input type="button" value="next" onclick="next1();"/>
	<h1>BBBBBBBBBBBBBBBBBBBBBBB</h1>
	<a href="c.html">BBBBBBBB</a>
	<script type="text/javascript">
		//到上一个页面
		function back1() {
			//history.back();
			history.go(-1);
		}
		//到下一个页面
		function next1() {
			//history.forward();
			history.go(1);
		}
	</script>
 </body>
```

## dom对象
### document对象
```html
<body>
	<input type="text" id="nameid" name="name1"/>
	<script type="text/javascript">
		var str = "abc";
		document.write(str);//向页面输出变量
		document.write("<hr/>");//输出html代码

		//使用getElementById得到input标签
		var input1 = document.getElementById("nameid");//传的id值
		alert(input1.name);//得到input里的name值
		input1.value = "bbbbb";//向input里面设置一个value值
		
		//使用getElementsByName得到input标签
		var inputs = document.getElementsByName("name1");//返回一个集合
		for(var i=0;i<inputs.length;i++) {
			var input1 = inputs[i];  //每次循环得到input对象
			alert(input1.value);    //得到每个input标签里面的value值
		}
	</script>
 </body>
```
### element对象
```html

```

### innerHTML
```html
<html>
 <head>
  <title>HTML示例</title>
  <style type="text/css">
		#div11 {
			width:200px;
			height:150px;
			border:2px dashed red;
		}
  </style>
 </head>
 <body>
	<span id="sid">哈哈呵呵</span>
	<div id="div11">
	</div>
	<script type="text/javascript">
		var span1 = document.getElementById("sid");
		alert(span1.innerHTML);//获取文本内容

		//向div里面设置内容 <h1>AAAAA</h1>
		var div11 = document.getElementById("div11");
		div11.innerHTML = "<h1>AAAAA</h1>";//设置内容

		//向div里面添加一个表格
		//var tab = "<table border='1'><tr><td>aaaaaa</td></tr><tr><td>bbbbbb</td></tr><tr><td>cccccc</td></tr></table>";
		var tab = "<table>";
		tab += "</table>";
		//相当于 var tab = "<table></table>";
		div11.innerHTML = tab;
	</script>

 </body>
</html>
```

## 练 习
### [练习1](#答案1)
使用js打印99乘法表

### [练习2](#练习2)
1、创建一个页面  
有两个输入项和一个按钮  
按钮上面有一个事件：弹出一个新窗口 open  
2、创建弹出页面  
  表格  
每一行有一个按钮和编号和姓名  
按钮上有一个事件：把当前的编号和姓名，赋值到第一个页面相应的位置  

### [练习3](#练习3)
在列表末尾添加一个节点

## 答案1
```html
<html>
 <head>
  <title>HTML示例</title>
  <style type="text/css">
  </style>
 </head>
 <body>
	<script type="text/javascript">
		document.write("<table border='1' bordercolor='blue'>");//document.write里面是双引号，如果设置标签的属性需要使用单引号
		//循环行 9
		for(var i=1;i<=9;i++) {
			document.write("<tr>");
			//循环列
			for(var j=1;j<=i;j++) {
				document.write("<td>");
				//运算
				document.write(j+"*"+i+"="+i*j);
				document.write("</td>");
			}
			//document.write("<br/>");
			document.write("</tr>");
		}
		document.write("</table>");
	</script>
 </body>
</html>
```

## 答案2
```html
<html>
 <head>
  <title>HTML示例</title>
  <style type="text/css">
  </style>
 </head>
 <body>
	编号：<input type="text" id="numid"/><br/>
	姓名：<input type="text" id="nameid"/><br/>
	<input type="button" value="选择" onclick="open1()"/>
	<script type="text/javascript">
		//实现弹出窗口的方法
		function open1() {
			//open方法
			window.open("user.html","","width=250,height=150");
		}
	</script>
 </body>
</html>

<html>
 <head>
  <title>HTML示例</title>
  <style type="text/css">
  </style>
 </head>
 <body>
	<table border="1" bordercolor="blue">
		<tr>
			<td>操作</td>
			<td>编号</td>
			<td>姓名</td>
		</tr>
		<tr>
			<td><input type="button" value="选择" onclick="s1('100','东方不败');"/></td>
			<td>100</td>
			<td>东方不败</td>
		</tr>
		<tr>
			<td><input type="button" value="选择" onclick="s1('101','岳不群');"/></td>
			<td>101</td>
			<td>岳不群</td>
		</tr>
		<tr>
			<td><input type="button" value="选择" onclick="s1('102','林平之');"/></td>
			<td>102</td>
			<td>林平之</td>
		</tr>
	</table>
	<script type="text/javascript">
		//实现s1方法
		function s1(num1,name1) {
			//需要把num1和name1赋值到window页面
			//跨页面的操作  opener：得到创建这个窗口的窗口 得到window页面
			var pwin = window.opener; //得到window页面
			pwin.document.getElementById("numid").value = num1;
			pwin.document.getElementById("nameid").value = name1;
			//关闭窗口
			window.close();
		}
	</script>
 </body>
</html>
```

## 答案3
```html
<html>
 <head>
  <title>HTML示例</title>
  <style type="text/css">
	div{
		width:200px;
		height:200px;
		border:2px solid red;
	}
  </style>
 </head>
 <body>
 <div>
	<ul id="ulid">
		<li>111</li>
		<li>222</li>
		<li>333</li>
		<li>444</li>
	</ul>
</div>
<div id="div1">

</div>
	<br/>
	<input type="button" value="add" onclick="add2();"/>
	<script type="text/javascript">
		function add2() {
			var div1 = document.getElementById("div1");
			var ul1 = document.getElementById("ulid");
			div1.appendChild(ul1);
		}

		//在末尾添加节点
		function add1() {
			//获取到ul标签
			var ul1 = document.getElementById("ulid");
			//创建标签
			var li1 = document.createElement("li");
			//创建文本
			var tex1 = document.createTextNode("5555");
			//把文本加入到li下面
			li1.appendChild(tex1);
			//把li加入到ul下面
			ul1.appendChild(li1);
		}
		
	</script>

 </body>
</html>
```
