# JavaScript
  - [基本语法](#基本语法)
  - [String](#string)
  - [Array](#array)
  - [Date](#date)
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
