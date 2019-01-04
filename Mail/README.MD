# JavaMail

## 发邮件
```java
public class Demo1 {
	public void fun1() throws Exception {
		/* 1. 得到session
		 * Session session = Session.getInstance(Properties prop, Authenticator auth); 
		 * 其中prop需要指定两个键值，一个是指定服务器主机名，另一个是指定是否需要认证！我们当然需要认证！ 
		 * 其中Authenticator是一个接口,表示认证器，即校验客户端的身份。
		 * 我们需要自己来实现这个接口，实现这个接口需要使用账户和密码。
		 */
		Properties props = new Properties();
		props.setProperty("mail.host", "smtp.163.com");//设置服务器主机名
		props.setProperty("mail.smtp.auth", "true");//设置需要认证		
		Authenticator auth = new Authenticator() {
			protected PasswordAuthentication getPasswordAuthentication() {
				return new PasswordAuthentication("itcast_cxf", "itcast");//用户名和密码
			}
		};
		Session session = Session.getInstance(props, auth);//得到session
		
		// 2. 创建MimeMessage
		MimeMessage msg = new MimeMessage(session);
		msg.setFrom(new InternetAddress("itcast_cxf@163.com"));//设置发件人
		msg.setRecipients(RecipientType.TO, "itcast_cxf@126.com");//设置收件人
		msg.setRecipients(RecipientType.CC, "itcast_cxf@sohu.com");//设置抄送
		msg.setRecipients(RecipientType.BCC, "itcast_cxf@sina.com");//设置暗送		
		msg.setSubject("这是来自ITCAST的测试邮件");//设置标题
		msg.setContent("这就是一封垃圾邮件！", "text/html;charset=utf-8");//设置邮件内容
		
		Transport.send(msg);//发送邮件
	}
```

## 发送带有附件的邮件
```java
	public void fun2() throws Exception {
		// 1. 得到session
		Properties props = new Properties();
		props.setProperty("mail.host", "smtp.163.com");
		props.setProperty("mail.smtp.auth", "true");
		Authenticator auth = new Authenticator() {
			protected PasswordAuthentication getPasswordAuthentication() {
				return new PasswordAuthentication("itcast_cxf", "itcast");
			}
		};
		Session session = Session.getInstance(props, auth);
		
		// 2. 创建MimeMessage
		MimeMessage msg = new MimeMessage(session);
		msg.setFrom(new InternetAddress("itcast_cxf@163.com"));//设置发件人
		msg.setRecipients(RecipientType.TO, "itcast_cxf@126.com");//设置收件人
		msg.setSubject("这是来自ITCAST的测试邮件有附件");//设置主题
		
		/* 当发送包含附件的邮件时，邮件体就为多部件形式！
		 * 1. 创建一个多部件的部件内容！MimeMultipart
		 *   MimeMultipart就是一个集合，用来装载多个主体部件！
		 * 2. 我们需要创建两个主体部件，一个是文本内容的，另一个是附件的。
		 *   主体部件叫MimeBodyPart
		 * 3. 把MimeMultipart设置给MimeMessage的内容！
		 */
		MimeMultipart list = new MimeMultipart();//创建多部件集合
		MimeBodyPart part1 = new MimeBodyPart();//创建第一个MimeBodyPart
		// 设置主体部件的内容
		part1.setContent("这是一封包含附件的垃圾邮件", "text/html;charset=utf-8");
		// 把主体部件添加到集合中
		list.addBodyPart(part1);
		
		MimeBodyPart part2 = new MimeBodyPart();//创建第二个MimeBodyPart
		part2.attachFile(new File("F:/f/白冰.jpg"));//设置附件的内容
		//设置显示的文件名称，其中encodeText用来处理中文乱码问题
		part2.setFileName(MimeUtility.encodeText("大美女.jpg"));
		list.addBodyPart(part2);
		
		msg.setContent(list);//把多部件作为邮件的内容。
		
		//发邮件
		Transport.send(msg);		
	}
```	

## MailUtils小工具
```java
	public void fun3() throws Exception {
		// 得到session
		Session session = MailUtils.createSession("smtp.163.com", 
				"itcast_cxf", "itcast");
		// 创建邮件对象
		Mail mail = new Mail("itcast_cxf@163.com",
				"itcast_cxf@126.com,itcast_cxf@sina.com",
				"不是垃圾邮件能是什么呢？", "这里是正文");
		
		// 创建两个附件对象
		AttachBean ab1 = new AttachBean(new File("F:/f/白冰.jpg"), "小美女.jpg");
		AttachBean ab2 = new AttachBean(new File("F:/f/big.jpg"), "我的羽绒服.jpg");
		
		// 添加到mail中
		mail.addAttach(ab1);
		mail.addAttach(ab2);
		
		// 发送邮件
		MailUtils.send(session, mail);
	}
}
```
