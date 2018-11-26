# html
  - [标签](#标签)




## 标签



### 列表标签
```html
<html>
 <head>
  <title>HTML示例</title>
 </head>
 <body>
	<!-- 列表标签 -->
	<dl>
		<dt>传智播客</dt>
		<dd>财务部</dd>
		<dd>学工部</dd>
		<dd>人事部</dd>
	</dl>

	<hr/>

	<!-- 有序列表 -->
	<ol type="i">
		<li>财务部</li>
		<li>学工部</li>
		<li>人事部</li>
	</ol>

	<hr/>
	<!-- 无序列表 -->
	<ul type="square">
		<li>财务部</li>
		<li>学工部</li>
		<li>人事部</li>
	</ul>
 </body>
</html>
```

### 表格标签
```html
<html>
 <head>
  <title>HTML示例</title>
 </head>
 <body>
	<table border="1" bordercolor="blue" cellspacing="0" width="400" height="150">
		<caption>人员信息</caption>
		<tr align="center">
			<td>姓名</td>
			<td>性别</td>
			<td>年龄</td>
		</tr>
		<tr>
			<td align="right">东方不败</td>
			<td>男</td>
			<td>20</td>
		</tr>
		<tr>
			<td>岳不群</td>
			<td>女</td>
			<td>30</td>
		</tr>
		<tr>
			<th>林平之</th>
			<th>男</th>
			<th>40</th>
		</tr>
	</table>
 </body>
</html>
```
### 表单标签
```html
<html>
 <head>
  <title>HTML示例</title>
 </head>
 <body>

	<form action="01-hello.html" method="get">
		手机号码：<input type="text" name="phone"/><br/>
		创建密码：<input type="password" name="pwd"/><br/>
		性别：<input type="radio" name="sex" value="nv" checked="checked"/>女 <input type="radio" name="sex" value="nan"/>男<br/>
		爱好：<input type="checkbox" name="love" value="y"/>羽毛球 <input type="checkbox" name="love" value="p"   checked="checked"/>乒乓球
			<input type="checkbox" name="love" value="pp"/> 排球<br/>
		文件：<input type="file"/><br/>
		生日：<select name="birth">
				<option value="0">请选择</option>
				<option value="1991" selected="selected">1991</option>
				<option value="1992">1992</option>
				<option value="1993">1993</option>
			</select>
		<br/>
		自我描述：<textarea cols="10" rows="10" name="tex"></textarea><br/>
		隐藏项：<input type="hidden" name="hid"/><br/>
		<input type="submit" value="注册"/>
		<input type="reset" value="重置注册"/>

		<input type="button" value="普通按钮"/>
		<br/>
		<!--  <input type="image" src="a.jpg"/>-->
	</form>
 </body>
</html>
```
