# EL表达式

  - [javabean导航](#javabean导航)



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
