# JSON

```java
window.onload = function() { 
	var str = "{\"name\": \"zhangSan\", \"age\": 18, \"sex\": \"male\"}";
	var person = eval("(" + str + ")"); //把一个字符串解析为js代码执行
	alert(person.name + ", " + person.age + ", " + person.sex);
}
```
