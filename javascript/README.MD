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
