# JSP
  - [3种java脚本](#3中java脚本)



## 3种java脚本
```jsp
<%
   int a = 10;//局部变量
%>
<%
 out.print(a++);//输出局部变量
%>
<br/>
<%=a %> //输出局部变量
<%!
int a = 100;//成员变量
public void fun1() { //定义方法
    System.out.println(a);
}
%>
<%
  out.print(this.a++);//输出成员变量
  fun1(); //调用方法
%>
```
