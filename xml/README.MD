# xml
  - xml的约束
    - [dtd约束](#dtd约束)
  - xml解析
    - [jaxp](#jaxp)
          
  - [练习](#练-习)
  

## xml的约束
### dtd约束
```xml
dtd文件：
<!ELEMENT person (name,age)>
<!ELEMENT name (#PCDATA)>
<!ELEMENT age (#PCDATA)>
xml文件：
<?xml version="1.0" encoding="utf-8"?>
<!DOCTYPE person SYSTEM "1.dtd">
<person>
	<name>zhangsan</name>
	<age>20</age>
	<a>1111</a>
</person>
```
* dtd引入方式
```xml
<!DOCTYPE 根元素名称 [
			<!ELEMENT person (name,age)>
			<!ELEMENT name (#PCDATA)>
			<!ELEMENT age (#PCDATA)>
		]>
```


## xml解析
### jaxp
```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<person>
	<p1>
		<name>zhangsan</name>
		<age>20</age>	
	</p1>
	<p1>
		<name>lisi</name>
		<age>30</age>
	</p1>
</person>
```
通过jaxp解析：
```java
public class TestJaxp {

	public static void main(String[] args) throws Exception {
		//selectAll();
		//selectSin();
		//addSex();
		//modifySex();
		//delSex();
		listElement();
	}

//查询所有name元素的值
	private static void selectAll() throws Exception {
		//查询所有name元素的值

		//创建解析器工厂    
		DocumentBuilderFactory builderFactory = DocumentBuilderFactory.newInstance();
		//创建解析器
		DocumentBuilder builder = builderFactory.newDocumentBuilder();
		//解析xml返回document
		Document document = builder.parse("src/person.xml");
		//得到name元素
		NodeList list = document.getElementsByTagName("name");
		for(int i=0;i<list.getLength();i++) {
			Node name1 = list.item(i); 
			//得到name元素里面的值
			String s = name1.getTextContent();
			System.out.println(s);
		}
	}
	
//查询xml中第一个name元素的值
	public static void selectSin() throws Exception {
		//创建解析器工厂
		DocumentBuilderFactory builderFactory = DocumentBuilderFactory.newInstance();
		//创建解析器
		DocumentBuilder builder = builderFactory.newDocumentBuilder();
		//解析xml，得到document
		Document document = builder.parse("src/person.xml");
		//得到所有的name元素
		NodeList list = document.getElementsByTagName("name");
		//使用下标 得到第一个元素
		Node name1 = list.item(1);
		//得到name里面的具体的值
		String s1 = name1.getTextContent();
		System.out.println(s1);
	}
	
//在第一个p1下面（末尾）添加 <sex>nv</sex>
	public static void addSex() throws Exception {
		//创建解析器工厂
		DocumentBuilderFactory builderFactory = DocumentBuilderFactory.newInstance();
		//创建解析器
		DocumentBuilder builder = builderFactory.newDocumentBuilder();
		//得到document
		Document document = builder.parse("src/person.xml");
		//得到所有的p1
		NodeList list = document.getElementsByTagName("p1");
		//得到第一个p1
		Node p1 = list.item(0);
		//创建标签
		Element sex1 = document.createElement("sex");
		//创建文本
		Text text1 = document.createTextNode("nv");
		//把文本添加到sex1下面
		sex1.appendChild(text1);
		//把sex1添加到p1下面
		p1.appendChild(sex1);
		//回写xml(把修改后的结果保存到文件中)
		TransformerFactory transformerFactory = TransformerFactory.newInstance();
		Transformer transformer = transformerFactory.newTransformer();
		//第一个参数表示源文件，第二个参数表示目的地文件路径
		transformer.transform(new DOMSource(document), new StreamResult("src/person.xml"));	
	}
	
//修改第一个p1下面的sex内容是nan
	public static void modifySex() throws Exception {
		DocumentBuilderFactory builderFactory = DocumentBuilderFactory.newInstance();
		//创建解析器
		DocumentBuilder builder = builderFactory.newDocumentBuilder();
		//得到document
		Document document = builder.parse("src/person.xml");
		//得到sex
		Node sex1 = document.getElementsByTagName("sex").item(0);
		//修改sex值
		sex1.setTextContent("nan");
		//回写xml
		TransformerFactory transformerFactory = TransformerFactory.newInstance();
		Transformer transformer = transformerFactory.newTransformer();
		transformer.transform(new DOMSource(document), new StreamResult("src/person.xml"));
	}
	
//删除<sex>nan</sex>节点
	public static void delSex() throws Exception {
		//创建解析器工厂
		DocumentBuilderFactory builderFactory = DocumentBuilderFactory.newInstance();
		//创建解析器
		DocumentBuilder builder = builderFactory.newDocumentBuilder();
		//得到document
		Document document = builder.parse("src/person.xml");
		//得到sex元素
		Node sex1 = document.getElementsByTagName("sex").item(0);
		//得到sex1父节点
		Node p1 = sex1.getParentNode();
		//删除操作
		p1.removeChild(sex1);
		//回写xml
		TransformerFactory transformerFactory = TransformerFactory.newInstance();
		Transformer transformer = transformerFactory.newTransformer();
		transformer.transform(new DOMSource(document), new StreamResult("src/person.xml"));
	}
	
//遍历节点，把所有元素名称打印出来
	public static void listElement() throws Exception {
		/*
		 * ====使用递归实现=====
		 * 得到根节点
		 * 得到根节点子节点
		 * 得到根节点子节点的子节点
		 */
		//创建解析器工厂
		DocumentBuilderFactory builderFactory = DocumentBuilderFactory.newInstance();
		//创建解析器
		DocumentBuilder builder = builderFactory.newDocumentBuilder();
		//得到document
		Document document = builder.parse("src/person.xml");	
		//编写一个方法实现遍历操作
		list1(document);
	}
	//递归遍历的方法
	private static void list1(Node node) {
		//判断是元素类型时候才打印
		if(node.getNodeType() == Node.ELEMENT_NODE) {			
			System.out.println(node.getNodeName());
		}	
		//得到一层子节点
		NodeList list = node.getChildNodes();
		//遍历list
		for(int i=0;i<list.getLength();i++) {
			//得到每一个节点
			Node node1 = list.item(i);
			//继续得到node1的子节点
			//node1.getChildNodes()
			list1(node1);
		}
	}
```


## 练 习
### [练习1](答案1)
根据DTD约束写xml文件


## 答案1
```xml
<!ELEMENT TVSCHEDULE (CHANNEL+)>

<!ELEMENT CHANNEL (BANNER,DAY+)>
<!ELEMENT BANNER (#PCDATA)>

<!ELEMENT DAY (DATE,(HOLIDAY|PROGRAMSLOT+)+)>
<!ELEMENT HOLIDAY (#PCDATA)>
<!ELEMENT DATE (#PCDATA)>

<!ELEMENT PROGRAMSLOT (TIME,TITLE,DESCRIPTION?)>
<!ELEMENT TIME (#PCDATA)>
<!ELEMENT TITLE (#PCDATA)> 
<!ELEMENT DESCRIPTION (#PCDATA)>

<!ATTLIST TVSCHEDULE NAME CDATA #REQUIRED>
<!ATTLIST CHANNEL CHAN CDATA #REQUIRED>
<!ATTLIST PROGRAMSLOT VTR CDATA #IMPLIED>
<!ATTLIST TITLE RATING CDATA #IMPLIED>
<!ATTLIST TITLE LANGUAGE CDATA #IMPLIED>


<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE TVSCHEDULE SYSTEM "TV.dtd">
<TVSCHEDULE NAME="CCTV">
	<CHANNEL CHAN="CCCC">
		<BANNER>aaaa</BANNER>
		<DAY>
			<DATE>2015-11-11</DATE>
			<HOLIDAY>xxxxx</HOLIDAY>
		</DAY>		
		<DAY>
			<DATE>2015-12-12</DATE>
			<PROGRAMSLOT VTR="SSSS">
				<TIME>1990</TIME>
				<TITLE>test</TITLE>
			</PROGRAMSLOT>
		</DAY>
	</CHANNEL>
</TVSCHEDULE>



```
