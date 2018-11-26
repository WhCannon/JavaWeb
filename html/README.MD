# html
  - [标签](#标签)
  - [练习](#练习)




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

## 练习
### [练习1](#答案1)
表单标签，注册界面案例




## 答案1
```html
<html>
 <head>
  <title>HTML示例</title>
 </head>
 <body>
	<h2>注册传智播客的账号</h2>
	<form action="01-hello.html">
	<table  width="100%">
		<tr>
			<td align="right">注册邮箱：</td>
			<td><input type="text" name="mail"/></td>
		</tr>
		<tr>
			<td>&nbsp;</td>
			<td>你可以使用 <a href="#">账号</a>注册或者使用 <a href="#">手机号</a>注册</td>
		</tr>
		<tr>
			<td align="right">创建密码：</td>
			<td><input type="password" name="pwd"/></td>
		</tr>
		<tr>
			<td align="right">真实姓名：</td>
			<td><input type="text" name="realname"/></td>
		</tr>
		<tr>
			<td align="right">性别：</td>
			<td><input type="radio" name="sex" value="nv"/>女 <input type="radio" name="sex" value="nan"/>男</td>
		</tr>

		<tr>
			<td  align="right" >生日：</td>
			<td>
				<select name="year">
					<option value="1945">1945</option>
					<option value="1931">1931</option>
					<option value="1949">1949</option>
				</select>年
				<select name="month">
					<option value="10">10</option>
					<option value="11">11</option>
					<option value="12">12</option>
				</select>月
				<select name="day">
					<option value="30">30</option>
					<option value="10">10</option>
					<option value="25">25</option>
				</select>日

			</td>
		</tr>

		<tr>
			<td  align="right">我现在：</td>
			<td>
				<select name="now">
					<option value="study">我正在上学</option>
					<option value="work">我已经工作</option>
				</select>

			</td>
		</tr>

		<tr>
			<td>&nbsp;</td>
			<td><img src="verycode.gif"/> <a href="#">看不清换一张?</a></td>
		</tr>

		<tr>
			<td align="right">验证码：</td>
			<td><input type="text" name="verycode"/></td>
		</tr>

		<tr>
			<td>&nbsp;</td>
			<td><input type="image" src="btn_reg.gif"/></td>
		</tr>
	</table>
</form>
 </body>
</html>

```
