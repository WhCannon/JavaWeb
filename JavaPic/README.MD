# Java生成图片


```java
public class Demo {
	public static void main(String[] args) throws FileNotFoundException, IOException {
		// 得到图片缓冲区
		BufferedImage bi = new BufferedImage(150, 70, BufferedImage.TYPE_INT_RGB);
		
		// 得到它的绘制环境（这张图片的笔）
		Graphics2D g2 = (Graphics2D) bi.getGraphics();
		
		g2.setColor(Color.WHITE);//设置颜色
		g2.fillRect(0, 0, 150, 70);//从指定的起点开始，填充整张图片(其实就是设置背景色)
		g2.setColor(Color.RED);
		g2.drawRect(0, 0, 150-1, 70-1);//画边框
		
		g2.setFont(new Font("宋体", Font.BOLD, 18));//设置字体（字体名称,字体样式,字体大小）
		g2.setColor(Color.BLACK);//设置颜色
		
		g2.drawString("HelloWorld", 3, 50);//向图片上写字符串
		ImageIO.write(bi, "JPEG", new FileOutputStream("F:/a.jpg"));//保存图片
	}
}
```
