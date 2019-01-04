# 文件上传与下载
## 文件上传
  - [上传的限制](#上传的限制)
  - [上传的步骤](#上传的步骤)
## [文件下载](#文件下载)

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
## 上传的注意事项
```java
public class Upload3Servlet extends HttpServlet {

	public void doPost(HttpServletRequest request, HttpServletResponse response)
			throws ServletException, IOException {
		request.setCharacterEncoding("utf-8");
		response.setContentType("text/html;charset=utf-8");

		// 得到工厂
		DiskFileItemFactory factory = new DiskFileItemFactory(20*1024, new File("F:/f/temp"));
		// 得到解析器
		ServletFileUpload sfu = new ServletFileUpload(factory);
		sfu.setFileSizeMax(100 * 1024);//限制单个文件大小为100K
		sfu.setSizeMax(1024 * 1024);//限制整个表单大小为1M
		
		try {
			List<FileItem> list = sfu.parseRequest(request);
			FileItem fi = list.get(1);			
			// 1. 得到文件保存的路径
			String root = this.getServletContext().getRealPath("/WEB-INF/files/");
			/* 2. 生成二层目录
			 *   1). 得到文件名称
			 *   2). 得到hashCode
			 *   3). 转发成16进制
			 *   4). 获取前二个字符用来生成目录 */
			String filename = fi.getName();//获取上传的文件名称
			// 处理文件名的绝对路径问题
			int index = filename.lastIndexOf("\\");
			if(index != -1) {
				filename = filename.substring(index+1);
			}
			// 给文件名称添加uuid前缀，处理文件同名问题
			String savename = CommonUtils.uuid() + "_" + filename;
			
			// 得到hashCode
			int hCode = filename.hashCode();
			String hex = Integer.toHexString(hCode);
			
			// 获取hex的前两个字母，与root连接在一起，生成一个完整的路径
			File dirFile = new File(root, hex.charAt(0) + "/" + hex.charAt(1));
			
			dirFile.mkdirs();// 创建目录链
			File destFile = new File(dirFile, savename);// 创建目录文件
			fi.write(destFile);//保存
		} catch (FileUploadException e) {
			if(e instanceof FileUploadBase.FileSizeLimitExceededException) {
				request.setAttribute("msg", "您上传的文件超出了100KB！");
				request.getRequestDispatcher("/form3.jsp").forward(request, response);
			}
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}
```


## 文件下载
```java
public class Download1Servlet extends HttpServlet {
	@Override
	public void doGet(HttpServletRequest req, HttpServletResponse resp)
			throws ServletException, IOException {
		/* 设置两个头一个流
		 * 1. Content-Type
		 * 2. Content-Disposition
		 * 3. 流：下载文件的数据 */
		String filename = "F:/流光飞舞.mp3";
		
		// 为了使下载框中显示中文文件名称不出乱码！
//		String framename = new String("流光%飞舞.mp3".getBytes("GBK"), "ISO-8859-1");
		String framename = filenameEncoding("流光%飞舞.mp3", req);
		
		String contentType = this.getServletContext()
				.getMimeType(filename);//通过文件名称获取MIME类型
		String contentDisposition = "attachment;filename=" + framename;
		// 一个流
		FileInputStream input = new FileInputStream(filename);
		
		//设置头
		resp.setHeader("Content-Type", contentType);
		resp.setHeader("Content-Disposition", contentDisposition);
		
		// 获取绑定了响应端的流
		ServletOutputStream output = resp.getOutputStream();		
		IOUtils.copy(input, output);//把输入流中的数据写入到输出流中。		
		input.close();
	}
	
	// 用来对下载的文件名称进行编码的！
	public static String filenameEncoding(String filename, HttpServletRequest request) throws IOException {
		String agent = request.getHeader("User-Agent"); //获取浏览器
		if (agent.contains("Firefox")) {
			BASE64Encoder base64Encoder = new BASE64Encoder();
			filename = "=?utf-8?B?"
					+ base64Encoder.encode(filename.getBytes("utf-8"))
					+ "?=";
		} else if(agent.contains("MSIE")) {
			filename = URLEncoder.encode(filename, "utf-8");
		} else {
			filename = URLEncoder.encode(filename, "utf-8");
		}
		return filename;
	}
}
```
