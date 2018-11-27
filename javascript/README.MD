# JavaScript
  - [基本语法](#基本语法)
  - [String](#string)
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
```


## 练习
### [练习1](#答案1)
使用js打印99乘法表

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
