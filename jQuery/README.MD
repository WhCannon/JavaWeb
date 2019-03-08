# JQuery

 - [百度搜索效果](#百度搜索效果)



## 百度搜索效果
搜索页面
```html
<body>
   	<center>
   	<h2>百度</h2>
   		<input type="text" name="word" id="word" style="width: 600px ; height: 50px  ;font-size: 20px;">
   		<input type="button" value="百度一下"  style="height: 55px ; width: 100px ; ">		
   		<div id="div01" style="position:relative; left : -54px; width: 600px; height: 200px ; border:  
                            1px solid blue; display: none"></div>
   	</center>
</body>
 ```
 JS请求
 ```java
 $(function(){
		$("#word").keyup(function() {
			// 获取输入框的值 
			var word = $(this).val();	
			if(word == ""){
				$("#div01").hide();
			}else{
				//3. 请求数据
				$.post("find",{word:word} ,function(data , status){
					//alert(data);
					$("#div01").show();
					$("#div01").html(data);
				});}	})});
 ```
 Servlet
 ```java
 public class FindWordsServlet extends HttpServlet{
   protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
	request.setCharacterEncoding("utf-8");
	try {
		//1. 先获取参数
		String word = request.getParameter("word");
		System.out.println("word="+word);
		//2. 让dao执行查询
		WordsDao dao = new WordsDaoImpl();
		List<WordBean> list = dao.findWords(word);
		for (WordBean wordBean : list) {
					System.out.println("==="+wordBean.toString());
				}				
		request.setAttribute("list", list);			
		//3. 返回数据
		response.setContentType("text/html;charset=utf-8");
		request.getRequestDispatcher("list.jsp").forward(request, response);
			} catch (SQLException e) {
				e.printStackTrace();
			}}
 ```
 list.jsp
 ```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<%@ taglib uri="http://java.sun.com/jsp/jstl/core"  prefix="c"%>
<table style="width: 100%">
	<c:forEach items="${list }" var="wordBean">
		<tr>
			<td>${wordBean.words}</td>
		</tr>
	</c:forEach>
</table>
 ```
 
