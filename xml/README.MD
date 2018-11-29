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
