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
 
 
