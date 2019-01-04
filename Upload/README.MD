# 文件上传与下载
## 文件上传
  - [上传的限制](#上传的限制)
  - [上传的步骤](#上传的步骤)


## 上传的限制
```java
<form action="xxx" method="post" enctype="multipart/form-data">
  用户名；<input type="text" name="username"/><br/>
  照　片：<input type="file" name="zhaoPian"/><br/>
  <input type="submit" value="上传"/>
</form>
```

## 上传的步骤
```java
public class Upload2Servlet extends HttpServlet {
	public void doPost(HttpServletRequest request, HttpServletResponse response)
			throws ServletException, IOException {
		request.setCharacterEncoding("utf-8");
		response.setContentType("text/html;charset=utf-8");
		
		/* 1. 得到工厂
		 * 2. 通过工厂创建解析器
		 * 3. 解析request，得到FileItem集合
		 * 4. 遍历FileItem集合，调用其API完成文件的保存*/
		DiskFileItemFactory factory = new DiskFileItemFactory();
		ServletFileUpload sfu = new ServletFileUpload(factory);
		try {
			List<FileItem> fileItemList = sfu.parseRequest(request);
			FileItem fi1 = fileItemList.get(0);
			FileItem fi2 = fileItemList.get(1);
			
			System.out.println("普通表单项演示：" + fi1.getFieldName() + 
					"=" + fi1.getString("UTF-8"));
			System.out.println("文件表单项演示：");
			System.out.println("Content-Type: " + fi2.getContentType());
			System.out.println("size: " + fi2.getSize());
			System.out.println("filename: " + fi2.getName());
			
			// 保存文件
			File destFile = new File("c:/baibing.jpg");
			fi2.write(destFile);
		} catch (FileUploadException e) {
			throw new RuntimeException(e);
		} catch (Exception e) {
			throw new RuntimeException(e);
		}
	}
}
```



## 文件下载
