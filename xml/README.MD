# xml
  - xml的约束
    - [dtd约束](#dtd约束)
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
