# xml
  - xml的约束
    - [dtd约束](#dtd约束)
    - [schema约束](#schema约束)
  - xml解析
    - [jaxp](#jaxp)
    - [dom4j](#dom4j)
          
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

### schema约束
```xml

<?xml version="1.0" encoding="UTF-8"?>
<person xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xmlns="http://www.itcast.cn/20151111"
xsi:schemaLocation="http://www.itcast.cn/20151111 1.xsd" id1="123">
	
	<name>zhangsan</name>
	<name>zhangsan</name>
	<name>zhangsan</name>
	<age>20</age> 
</person>


<?xml version="1.0" encoding="UTF-8"?>
<schema xmlns="http://www.w3.org/2001/XMLSchema"
targetNamespace="http://www.itcast.cn/20151111"
elementFormDefault="qualified">
	<element name="person">
		<complexType>
			<sequence>
			<!-- <all> -->
			<!-- <choice> -->
					<element name="name" type="string" maxOccurs="unbounded"></element>
					<element name="age" type="int"></element>
			<!-- </choice> -->
			<!-- </all>	 -->
			</sequence>
			<attribute name="id1" type="int" use="required"></attribute>
		</complexType>
	</element>
</schema>
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
## dom4j
* 得到Document对象：
```java
SAXReader reader = new SAXReader();
Document document = reader.read(url);
```
### 使用dom4j对xml进行增删改查
* xml文件
```xml
<?xml version="1.0" encoding="UTF-8"?>
<person> 
  <p1 id1="aaaa"> 
    <name>zhangsan</name>  
    <age>300</age>  
    <sex>nv</sex> 
  </p1>  
  <p1> 
    <name>lisi</name>  
    <age>30</age> 
  </p1> 
</person>
```
* 解析过程
```java
//查询xml中所有name元素的值
	public static void selectName() throws Exception {
		//创建解析器
		SAXReader saxReader = new SAXReader();
		//得到document
		Document document = saxReader.read("src/p1.xml");
		//得到根节点
		Element root = document.getRootElement();
		
		//得到p1
		List<Element> list = root.elements("p1");
		//遍历list
		for (Element element : list) {
			//得到p1下面的name元素
			Element name1 = element.element("name");
			//得到name里面的值
			String s = name1.getText();
			System.out.println(s);
		}
	}
//在第一个p1标签末尾添加一个元素 <sex>nv</sex>	
public static void addSex() throws Exception {
		//创建解析器
//		SAXReader reader = new SAXReader();
		//得到document
//		Document document = reader.read("src/p1.xml");
		Document document = Dom4jUtils.getDocument(Dom4jUtils.PATH);
		//得到根节点
		Element root = document.getRootElement();
		//得到第一个p1元素
		Element p1 = root.element("p1");
		//在p1下面直接添加元素
		Element sex1 = p1.addElement("sex");
		//在sex下面添加文本
		sex1.setText("nv");
		
		//回写xml
//		OutputFormat format = OutputFormat.createPrettyPrint(); //写入格式，带缩进的效果
//		OutputFormat format = OutputFormat.createCompactFormat();
//		XMLWriter  xmlWriter = new XMLWriter(new FileOutputStream("src/p1.xml"), format);
//		xmlWriter.write(document);
//		xmlWriter.close();
		Dom4jUtils.xmlWriters(Dom4jUtils.PATH, document);
	}
	
//在第一个p1下面的age标签之前添加 <school>ecit.edu.cn</schlool>
	public static void addAgeBefore() throws Exception {
		//创建解析器
//		SAXReader saxReader = new SAXReader();
		//得到document
//		Document document = saxReader.read("src/p1.xml");
		
		Document document = Dom4jUtils.getDocument(Dom4jUtils.PATH);
		//得到根节点
		Element root = document.getRootElement();
		//获取到第一个p1
		Element p1 = root.element("p1");
		//获取p1下面的所有元素
		List<Element> list = p1.elements();
		//创建元素 使用
		Element school = DocumentHelper.createElement("school");
		//在school下面创建文本
		school.setText("ecit");	
		//在特定位置添加
		list.add(1, school);
		//回写xml
		/*OutputFormat format = OutputFormat.createPrettyPrint();
		XMLWriter xmlWriter = new XMLWriter(new FileOutputStream("src/p1.xml"), format);
		xmlWriter.write(document);
		xmlWriter.close();*/
		Dom4jUtils.xmlWriters(Dom4jUtils.PATH, document);
	}
//dom4j里封装方法	
public class Dom4jUtils {	
	public static final String PATH = "src/p1.xml";
	//返回document
	public static Document getDocument(String path) {
		try {
			//创建解析器
			SAXReader reader = new SAXReader();
			//得到document
			Document document = reader.read(path);
			return document;
		} catch (Exception e) {
			e.printStackTrace();
		}
		return null;
	}
	
	//回写xml的方法
	public static void xmlWriters(String path,Document document) {
		try {
			OutputFormat format = OutputFormat.createPrettyPrint();
			XMLWriter xmlWriter = new XMLWriter(new FileOutputStream(path), format);
			xmlWriter.write(document);
			xmlWriter.close();
		}catch(Exception e) {
			e.printStackTrace();
		}
	}
}

//修改第一个p1下面的age元素的值 <age>30</age>
	public static void modifyAge() throws Exception {
		Document document = Dom4jUtils.getDocument(Dom4jUtils.PATH);
		Element root = document.getRootElement();
		Element p1 = root.element("p1");
		//得到p1下面的age
		Element age = p1.element("age");
		//修改age的值
		age.setText("300");
		Dom4jUtils.xmlWriters(Dom4jUtils.PATH, document);
	}
	
//删除第一个p1下面的<school>ecit</school>元素
	public static void delSch() throws Exception {
		Document document = Dom4jUtils.getDocument(Dom4jUtils.PATH);
		Element root = document.getRootElement();
		Element p1 = root.element("p1");
		//得到p1下面的school标签
		Element sch = p1.element("school");
		//删除school元素，通过父节点删除 
		p1.remove(sch);
		Dom4jUtils.xmlWriters(Dom4jUtils.PATH, document);
	}
	
//获取第一个p1里面的属性id1的值
	public static void getValues() throws Exception {
		Document document = Dom4jUtils.getDocument(Dom4jUtils.PATH);
		Element root = document.getRootElement();
		Element p1 = root.element("p1");
		//得到p1里面的属性值
		String value = p1.attributeValue("id1");
		System.out.println(value);
	}
	

```

## 练 习
### [练习1](#答案1)
根据DTD约束写xml文件

### [练习2](#答案2)
实现学生管理系统，使用xml存储数据

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

## 答案2
* 学生类（省略）
* 学生数据xml
```xml
<?xml version="1.0" encoding="UTF-8"?>
<sudent> 
  <stu> 
    <id>100</id>  
    <name>zhangsan</name>  
    <age>20</age> 
  </stu>  
  <stu> 
    <id>101</id>  
    <name>lisi</name>  
    <age>30</age> 
  </stu> 
</sudent>
```
* 具体操作类
```java
public class StuService {
	//查询 根据id查询学生信息
	public static Student getStu(String id) throws Exception {
		//创建解析器
		SAXReader saxReader = new SAXReader();
		//得到document
		Document document = saxReader.read("src/student.xml");
		//获取所有的id
		List<Node> list = document.selectNodes("//id");
		//创建student对象
		Student student = new Student();
		//遍历list
		for (Node node : list) { //node是每一个id节点
			//得到id节点的值
			String idv = node.getText();
			//判断id是否相同
			if(idv.equals(id)) { 
				//得到id的父节点 stu
				Element stu = node.getParent();
				//通过stu获取name和age
				String namev = stu.element("name").getText();
				String agev = stu.element("age").getText();
				student.setId(idv);
				student.setName(namev);
				student.setAge(agev);
			}
		}
		return student;
	}
	
	//增加
	public static void addStu(Student student) throws Exception {
		//创建解析器
		SAXReader saxReader = new SAXReader();
		//得到document
		Document document = saxReader.read("src/student.xml");
		//得到根节点
		Element root = document.getRootElement();
		//在根节点上面添加stu
		Element stu = root.addElement("stu");
		//在stu标签上面依次添加id name age标签
		Element id1 = stu.addElement("id");
		Element name1 = stu.addElement("name");
		Element age1 = stu.addElement("age");
		//在id name age上面依次添加值
		id1.setText(student.getId());
		name1.setText(student.getName());
		age1.setText(student.getAge());
		//回写xml
		OutputFormat format = OutputFormat.createPrettyPrint();
		XMLWriter xmlWriter = new XMLWriter(new FileOutputStream("src/student.xml"), format);
		xmlWriter.write(document);
		xmlWriter.close();
	}
	
	//删除 根据学生的id删除
	public static void delStu(String id) throws Exception {
		//创建解析器
		SAXReader saxReader = new SAXReader();
		//得到document
		Document document = saxReader.read("src/student.xml");
		//获取所有的id  xpath: //id
		List<Node> list = document.selectNodes("//id");
		//遍历list集合
		for (Node node : list) { //node是每一个id的元素
			//得到id的值
			String idv = node.getText();
			//判断idv和传递的id是否相同
			if(idv.equals(id)) { //id相同
				//得到stu节点
				Element stu = node.getParent();
				//获取stu的父节点
				Element student = stu.getParent();
				//删除stu
				student.remove(stu);
			}		
		}
		//回写xml
		OutputFormat format = OutputFormat.createPrettyPrint();
		XMLWriter xmlWriter = new XMLWriter(new FileOutputStream("src/student.xml"), format);
		xmlWriter.write(document);
		xmlWriter.close();
	}
}
```
