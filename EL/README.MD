# EL表达式
  - [javabean导航](#javabean导航)
  - [EL函数库](#EL函数库)
# JSTL
  - [core标签库](#core标签库)


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
