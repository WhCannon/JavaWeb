# EL表达式
  - [javabean导航](#javabean导航)
  - [EL函数库](#EL函数库)
# JSTL
  - [core标签库](#core标签库)
# [自定义标签](#自定义标签)


## javabean导航
```jsp
<%
	
	Employee emp = new Employee();
	emp.setName("李小四");
	emp.setSalary(123456);
	emp.setAddress(address);	
	request.setAttribute("emp", emp);
%>
<h3>使用el获取request域的emp</h3>
${requestScope.emp.address.street }
<!-- request.getAttribute("emp").getAddress().getStreet() --><br/>
```

## EL函数库
```java
<%@taglib prefix="fn" uri="http://java.sun.com/jsp/jstl/functions" %>
<%
String[] strs = {"a", "b","c"};
pageContext.setAttribute("arr", strs);
%>
${fn:length(arr) }<br/>
${fn:substringAfter("Hello-World", "-")}<br/><!-- World -->
${fn:substringBefore("Hello-World", "-")}<br/><!-- Hello -->
${fn:escapeXml("<html></html>")}<br/> <!-- <html></html> -->

```

## core标签库
* foreach
```jsp
<c:forEach var="i" begin="1" end="10">
    ${i}
</c:forEach>
相当于
for(int i = 1; i <= 10; i++) {
    ...
  }

遍历数组、集合
<c:forEach items="${strs }" var="str">
 ${str }<br/>
</c:forEach>
等同于
for(String str : strs) {
  ...
}
```

## 自定义标签
标签处理类
```java
/**
 * SimpleTagSupport它实现了SimpleTag接口。
 *   它已经把所有的tomcat传递的数据都保存起来了！而且还提供了get方法供子类调用！
 * @author cxf
 *
 */
public class MyTag2 extends SimpleTagSupport {
	@Override
	public void doTag() throws JspException, IOException {
		this.getJspContext().getOut().print("再 Hello 一次 Tag!");
	}
}

```
tld文件
```jsp
  <tag>
  	<name>myTag1</name> 指定当前标签的名称
  	<tag-class>cn.itcast.tag.MyTag1</tag-class> 指定当前标签的标签处理类！
  	<body-content>empty</body-content> 指定标签体的类型，我们这里使用的是空标签！
  </tag>

```
