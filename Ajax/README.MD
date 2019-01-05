# Ajax
  - [Ajax第一例](#Ajax第一例)
  - [校验用户名是否被注册](#例2)
  - [响应内容为xml](#例3)
  - [省市联动](#例4)
  - [XStream](#xstream)
  

## Ajax第一例
* Servlet
```java
public class AServlet extends HttpServlet {
	public void doGet(HttpServletRequest request, HttpServletResponse response)
			throws ServletException, IOException {
		System.out.println("Hello AJAX!");
		response.getWriter().print("Hello AJAX!!!");
	}}
```
* jsp
```jsp
<html>
<head>
...

<script type="text/javascript">
// 创建异步对象，需要先判断浏览器的版本
function createXMLHttpRequest() {
	try {
		return new XMLHttpRequest();//大多数浏览器
	} catch (e) {
		try {
			return ActvieXObject("Msxml2.XMLHTTP");//IE6.0
		} catch (e) {
			try {
				return ActvieXObject("Microsoft.XMLHTTP");//IE5.5及更早版本	
			} catch (e) {
				alert("哥们儿，您用的是什么浏览器啊？");
				throw e;
			}}}}

window.onload = function() {//监听器，文档加载完毕后执行
	var btn = document.getElementById("btn");//获取按钮
	btn.onclick = function() {//给按钮的点击事件注册监听
    //ajax四步操作，得到服务器的响应，把响应结果显示到h1元素中
    
		var xmlHttp = createXMLHttpRequest();//1.得到异步对象
		// 2.打开与服务器的连接
		xmlHttp.open("GET", "<c:url value='/AServlet'/>", true);
		// 3. 发送请求
		xmlHttp.send(null);//GET请求没有请求体，但也要给出null，不然FireFox可能无法发送请求！
		// 4. 给异步对象的onreadystatechange事件注册监听器
		xmlHttp.onreadystatechange = function() {//当xmlHttp的状态发生变化时执行
			// 双重判断：xmlHttp的状态为4（服务器响应结束），以及服务器响应的状态码为200（响应成功）
			if(xmlHttp.readyState == 4 && xmlHttp.status == 200) {
				// 获取服务器的响应内容
				var text = xmlHttp.responseText;
				// 获取h1元素
				var h1 = document.getElementById("h1");
				h1.innerHTML = text;
			}};};};
</script>
  </head>
  
  <body>
<button id="btn">点击这里</button>
<h1 id="h1"></h1>
  </body>
</html>
```

## 例2:
注册表单之校验用户是否注册
```java
1. 编写页面：ajax3.jsp
    > 给出注册表单页面
    > 给用户名文本框添加onblur事件的监听
    > 获取文本框的内容，通过ajax发送给服务器，得到响应结果
      * 如果为1：在文本框后显示“用户名已被注册”
      * 如果为0：什么都不做！

2. 编写Servlet: ValidateUsernameServlet
    > 获取客户端传递的用户名参数
    > 判断是否为itcast
      * 是：返回1
      * 否：返回0
```
* jsp
```jsp
<html>
<head>
...

<script type="text/javascript">
判断浏览器版本，省略。。。
window.onload = function() {
	// 获取文本框，给它的失去焦点事件注册监听
	var userEle = document.getElementById("usernameEle");
	userEle.onblur = function() {
		//1.得到异步对象
		var xmlHttp = createXMLHttpRequest();
		//2.打开连接
		xmlHttp.open("POST", "<c:url value='/ValidateUsernameServlet'/>", true);
		//3.设置请求头：Content-Type
		xmlHttp.setRequestHeader("Content-Type", "application/x-www-form-urlencoded");
		//4.发送请求，给出请求体
		xmlHttp.send("username=" + userEle.value);
		
		//5.给xmlHttp的onreadystatechange事件注册监听
		xmlHttp.onreadystatechange = function() {
			if(xmlHttp.readyState == 4 && xmlHttp.status == 200) {//双重判断
				//获取服务器的响应，判断是否为1
				// 是：获取span，添加内容：“用户名已被注册”
				var text = xmlHttp.responseText;
				var span = document.getElementById("errorSpan");//得到span元素
				if(text == "1") {
					span.innerHTML = "用户名已被注册！";
				} else {
					span.innerHTML = "";
				}}};};};
</script>
  </head>
  
  <body>
<h1>演示用户名是否被注册</h1>
<form action="" method="post">
用户名：<input type="text" name="username" id="usernameEle"/><span id="errorSpan"></span><br/>
密　码：<input type="password" name="password"/><br/>
<input type="submit" value="注册"/>
</form>
  </body>
</html>
```
* Servlet
```java
public class ValidateUsernameServlet extends HttpServlet {
	public void doPost(HttpServletRequest request, HttpServletResponse response)
			throws ServletException, IOException {
		request.setCharacterEncoding("utf-8");
		response.setContentType("text/html;charset=utf-8");
		
		/* 1. 获取参数username
		 * 2. 判断是否为itcast
		 *    如果是：响应1
		 *    如果不是：响应0 */
		String username = request.getParameter("username");
		if(username.equalsIgnoreCase("itcast")) {
			response.getWriter().print("1");
		} else {
			response.getWriter().print("0");
		}}}
```
## 例3
响应内容为xml
* Servlet
```java
public class BServlet extends HttpServlet {
	public void doGet(HttpServletRequest request, HttpServletResponse response)
			throws ServletException, IOException {
		String xml = "<students>" +
			"<student number='ITCAST_1001'>" +
			"<name>zhangSan</name>" +
			"<age>18</age>" + 
			"<sex>male</sex>" +
			"</student>" +
			"</students>";		
		response.setContentType("text/xml;charset=utf-8");
		response.getWriter().print(xml);
	}
}
```
* jsp
```jsp
<html>
<head>
...

<script type="text/javascript">
判断浏览器版本，省略。。。
window.onload = function() {//文档加载完毕后执行
	var btn = document.getElementById("btn");
	btn.onclick = function() {//给按钮的点击事件注册监听
		var xmlHttp = createXMLHttpRequest();
		xmlHttp.open("GET", "<c:url value='/BServlet'/>", true);
		xmlHttp.send(null);//GET请求没有请求体，但也要给出null，不然FireFox可能会不能发送！
		xmlHttp.onreadystatechange = function() {//当xmlHttp的状态发生变化时执行
			// 双重判断：xmlHttp的状态为4（服务器响应结束），以及服务器响应的状态码为200（响应成功）
			if(xmlHttp.readyState == 4 && xmlHttp.status == 200) {
				// 获取服务器的响应结果（xml）
				var doc = xmlHttp.responseXML;
				// 查询文档下名为student的所有元素，得到数组，再取下标0元素
				var ele = doc.getElementsByTagName("student")[0];
				var number = ele.getAttribute("number");//获取元素名为number的属性值
				var name;
				var age;
				var sex;
				
				// 处理浏览器的差异
				if(window.addEventListener) {
					name = ele.getElementsByTagName("name")[0].textContent;//其他浏览器
				} else {
					name = ele.getElementsByTagName("name")[0].text;//IE支持
				}
				if(window.addEventListener) {
					age = ele.getElementsByTagName("age")[0].textContent;//其他浏览器
				} else {
					age = ele.getElementsByTagName("age")[0].text;//IE支持
				}
				if(window.addEventListener) {
					sex = ele.getElementsByTagName("sex")[0].textContent;//其他浏览器
				} else {
					sex = ele.getElementsByTagName("sex")[0].text;//IE支持
				}

				var text = number + ", " + name + ", " + age + ", " + sex;
				document.getElementById("h1").innerHTML = text;
			}
		};
	};
};
</script>
  </head>
  
  <body>
<button id="btn">点击这里</button>
<h1 id="h1"></h1>
  </body>
</html>
```

## 例4
省市联动
* ProvinceServlet
```java
public class ProvinceServlet extends HttpServlet {

	public void doGet(HttpServletRequest request, HttpServletResponse response)
			throws ServletException, IOException {
		response.setContentType("text/html;charset=utf-8");
		// 响应所有省份名称，使用逗号分隔！
		// 1. 得到Document对象
		// 创建解析器对象,调用解析器的读方法，传递一个流对象，得到Document
		try {
			SAXReader reader = new SAXReader();
			InputStream input = this.getClass().getResourceAsStream("/china.xml");
			Document doc = reader.read(input);
			
			/* 查询所有province的name属性，得到一堆的属性对象
			 * 循环遍历，把所有的属性值连接成一个字符串，发送给客户端 */
			List<Attribute> arrList = doc.selectNodes("//province/@name");
			StringBuilder sb = new StringBuilder();
			for(int i = 0; i < arrList.size(); i++) {
				sb.append(arrList.get(i).getValue());//把每个属性的值存放到sb中。
				if(i < arrList.size() - 1) {
					sb.append(",");
				}
			}
			response.getWriter().print(sb);
		} catch(Exception e) {
			throw new RuntimeException(e);
		}
	}
}
```
* CityServlet
```java
public class CityServlet extends HttpServlet {
	public void doPost(HttpServletRequest request, HttpServletResponse response)
			throws ServletException, IOException {
		request.setCharacterEncoding("utf-8");
		response.setContentType("text/xml;charset=utf-8");//注意：发送xml这里要修改！！！
		
		/* 1. 获取省份的名称
		 * 2. 使用省份名称查找到对应的<province>元素
		 * 3. 把<province>元素转换成字符串，发送！*/
		try {// 得到Document
			SAXReader reader = new SAXReader();
			InputStream input = this.getClass().getResourceAsStream("/china.xml");
			Document doc = reader.read(input);

			String pname = request.getParameter("pname");//获取省份名称
			Element proEle = (Element) doc.selectSingleNode("//province[@name='" + pname + "']");//[@name="北京"]
			String xmlStr = proEle.asXML();//把元素转换成字符串
			response.getWriter().print(xmlStr);
		} catch(Exception e) {
			throw new RuntimeException(e);
		}
	}
}
```
* jsp
```jsp
<html>
<head>
...

<script type="text/javascript">
判断浏览器版本，省略。。。
/* 1. 在文档加载完毕时发送请求，得到所有省份名称，显示在<select name="province"/>中
 * 2. 在选择了新的省份时，发送请求（参数为省名称），得到xml文档，即<province>元素
 *   解析xml文档，得到其中所有的<city>，再得到每个<city>元素的内容，即市名
               使用市名生成<option>，插入到<select name="city">元素中 */
window.onload = function() {
	//ajax四步，请求ProvinceServlet，得到所有省份名称
	//使用每个省份名称创建一个<option>元素，添加到<select name="province">中
	var xmlHttp = createXMLHttpRequest();
	xmlHttp.open("GET", "<c:url value='/ProvinceServlet'/>", true);
	xmlHttp.send(null);
	xmlHttp.onreadystatechange = function() {
		if(xmlHttp.readyState == 4 && xmlHttp.status == 200) {
			var text = xmlHttp.responseText;//获取服务器的响应
			var arr = text.split(",");//使用逗号分隔它，得到数组
			// 循环遍历每个省份名称，每个名称生成一个option对象，添加到<select>中
			for(var i = 0; i < arr.length; i++) {
				var op = document.createElement("option");//创建一个指名名称元素
				op.value = arr[i];//设置op的实际值value为当前的省份名称
				var textNode = document.createTextNode(arr[i]);//创建文本节点
				op.appendChild(textNode);//把文本子节点添加到op元素中，指定其显示值				
				document.getElementById("p").appendChild(op);
			}}};
	
	/* 第二件事情：给<select name="province">添加改变监听
	         使用选择的省份名称请求CityServlet，得到<province>元素(xml元素)！！！
	         获取<province>元素中所有的<city>元素，遍历之！获取每个<city>的文本内容，即市名称
	         使用每个市名称创建<option>元素添加到<select name="city"> */
	var proSelect = document.getElementById("p");
	proSelect.onchange = function() {
		var xmlHttp = createXMLHttpRequest();
		xmlHttp.open("POST", "<c:url value='/CityServlet'/>", true);
		xmlHttp.setRequestHeader("Content-Type", "application/x-www-form-urlencoded");
		xmlHttp.send("pname=" + proSelect.value);//把下拉列表中选择的值发送给服务器！
		xmlHttp.onreadystatechange = function() {
			if(xmlHttp.readyState == 4 && xmlHttp.status == 200) {
				//把select中的所有option移除（除了请选择）
				var citySelect = document.getElementById("c");
				// 获取其所有子元素
				var optionEleList = citySelect.getElementsByTagName("option");
				// 循环遍历每个option元素，然后在citySelect中移除
				while(optionEleList.length > 1) {//子元素的个数如果大于1就循环，等于1就不循环了！
					citySelect.removeChild(optionEleList[1]);//总是删除1下标，因为1删除了，2就变成1了！
				}
				var doc = xmlHttp.responseXML;
				// 得到所有名为city的元素
				var cityEleList = doc.getElementsByTagName("city");
				// 循环遍历每个city元素
				for(var i = 0; i < cityEleList.length; i++) {
					var cityEle = cityEleList[i];//得到每个city元素
					var cityName;
					// 获取市名称
					if(window.addEventListener) {//处理浏览器的差异
						cityName = cityEle.textContent;//支持FireFox等浏览器
					} else {
						cityName = cityEle.text;//支持IE
					}
					// 使用市名称创建option元素，添加到<select name="city">中
					var op = document.createElement("option");
					op.value = cityName;
					// 创建文本节点
					var textNode = document.createTextNode(cityName);
					op.appendChild(textNode);//把文本节点追加到op元素中
					//把op添加到<select>元素中
					citySelect.appendChild(op);
				}}};};};
</script>
  </head>
  
  <body>
<h1>省市联动</h1>
<select name="province" id="p">
  <option>===请选择省===</option>
</select>
　　　
<select name="city" id="c">
  <option>===请选择市===</option>
</select>
  </body>
</html>
```

## XStream
```java
public class Demo1 {
	// 返回javabean集合
	public List<Province> getProinvceList() {
		Province p1 = new Province();
		p1.setName("北京");
		p1.addCity(new City("东城区", "DongChengQu"));
		p1.addCity(new City("昌平区", "ChangPingQu"));
		
		Province p2 = new Province();
		p2.setName("辽宁");
		p2.addCity(new City("沈阳", "shenYang"));
		p2.addCity(new City("葫芦岛", "huLuDao"));
		
		List<Province> provinceList = new ArrayList<Province>();
		provinceList.add(p1);
		provinceList.add(p2);
		return provinceList;
	}
public void fun1() {
		List<Province> proList = getProinvceList();
		/* 创建XStream对象
		 * 调用toXML()把集合转换成xml字符串 */
		XStream xstream = new XStream();
		String s = xstream.toXML(proList);
		System.out.println(s);
	}
转换完的格式：
<list> --> 给的List类型显示list
  <cn.itcast.demo1.Province> --> javabean的类型为Province，它元素的名称为类的完整名
    <name>北京</name> --> javabean的属性名
    <cities> --> javabean的属性名
      <cn.itcast.demo1.City> --> 类名
        <name>东城区</name> --> 属性名
        <description>DongChengQu</description> --> 属性名
      </cn.itcast.demo1.City>
      <cn.itcast.demo1.City>
        <name>昌平区</name>
        <description>ChangPingQu</description>
      </cn.itcast.demo1.City>
    </cities>
  </cn.itcast.demo1.Province>
	...
	...
</list>

/* 别名(alias)
* 默认List类型是对应<list>元素，希望让List类型对应<china>元素
* 默认Province类型是对应<cn.itcast.demo1.Province>，希望让它对应<province>
* 默认City类型是对应<cn.itcast.demo1.City>，希望它对应<city>元素 */

public void fun2() {
		List<Province> proList = getProinvceList();
		XStream xstream = new XStream();
		// 给指定的类型指定别名
		xstream.alias("china", List.class);//给List类型指定别名为china
		xstream.alias("province", Province.class);//给Province指定别名为province
		xstream.alias("city", City.class);//给City类型指定别名为city		
		String s = xstream.toXML(proList);
		System.out.println(s);
	}
指定完别名的格式是：
<china>
  <province>
    <name>北京</name>
    <cities>
      <city>
        <name>东城区</name>
        <description>DongChengQu</description>
      </city>
      <city>
        <name>昌平区</name>
        <description>ChangPingQu</description>
      </city>
    </cities>
  </province>
</china>

默认javabean属性都会生成一个子元素，而现在希望直接在父标签上生成元素的属性
就是说，想把name放到<Province>标签上
public void fun3() {
		List<Province> proList = getProinvceList();
		XStream xstream = new XStream();
		xstream.alias("china", List.class);//给List类型指定别名为china
		xstream.alias("province", Province.class);//给Province指定别名为province
		xstream.alias("city", City.class);//给City类型指定别名为city
		
		// 把Province类型的name属性，生成<province>元素的属性
		xstream.useAttributeFor(Province.class, "name");
		String s = xstream.toXML(proList);
		System.out.println(s);		
	}
<china>
  <province name="北京">
    <cities>
      <city>
        <name>东城区</name>
        <description>DongChengQu</description>
      </city>
      <city>
        <name>昌平区</name>
        <description>ChangPingQu</description>
      </city>
    </cities>
  </province>
 </china>
  
去除List类型的属性，只把list中的元素生成xml元素
不显示List属性，只显示里面的元素
去除<cities>，因为没啥用
public void fun4() {
		List<Province> proList = getProinvceList();
		XStream xstream = new XStream();
		xstream.alias("china", List.class);//给List类型指定别名为china
		xstream.alias("province", Province.class);//给Province指定别名为province
		xstream.alias("city", City.class);//给City类型指定别名为city
		xstream.useAttributeFor(Province.class, "name");//把Province类型的name属性，生成<province>元素的属性
		
		/* 去除<cities>这样的Collection类型的属性
		 * 去除Provice类的名为cities的List类型的属性！*/
		xstream.addImplicitCollection(Province.class, "cities");
		String s = xstream.toXML(proList);
		System.out.println(s);		
	}
<china>
  <province name="北京">
    <city>
      <name>东城区</name>
      <description>DongChengQu</description>
    </city>
    <city>
      <name>昌平区</name>
      <description>ChangPingQu</description>
    </city>
  </province>
</china>

/* 去除不想要的javabean属性
 * 就是让某些javabean属性不生成对应的xml元素！*/
public void fun5() {
		List<Province> proList = getProinvceList();
		XStream xstream = new XStream();
		xstream.alias("china", List.class);//给List类型指定别名为china
		xstream.alias("province", Province.class);//给Province指定别名为province
		xstream.alias("city", City.class);//给City类型指定别名为city
		xstream.useAttributeFor(Province.class, "name");//把Province类型的name属性，生成<province>元素的属性
		xstream.addImplicitCollection(Province.class, "cities");//去除Provice类的名为cities的List类型的属性！
		
		// 让City类的，名为description属性不生成对应的xml元素
		xstream.omitField(City.class, "description");
		String s = xstream.toXML(proList);
		System.out.println(s);		
	}

<china>
  <province name="北京">
    <city>
      <name>东城区</name>
    </city>
    <city>
      <name>昌平区</name>
    </city>
</china>
```
