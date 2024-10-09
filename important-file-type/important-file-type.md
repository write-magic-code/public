# 重要文件类型

了解流行文件类型，并清楚会在哪里遇到它们。

## json

人们乐于用 json 发送数据，无论是内存间，还是网络发包，甚至是数据库存储（mongodb）。当然，人们还喜欢用 json 当配置文件。

json 是基于 JavaScript 的一个子集设计的，是一种开放标准的文件格式和数据交换格式，它易于人阅读和编写，同时也易于机器解析和生成。json 独立于语言设计，很多编程语言都支持 json 格式的数据交换。

json 主要支持以下类型：  
- string, number, boolean, null（值） 
- array, object（容器） 

json 是严格的，**只容忍多余的空格空行**。除了 object 必须由 key value 构成，且 key 必须是 string 外，没有任何要求。  

一个 json 只能是一个表达式，不能为多。  
* .json
```json
{
    "name": "Tom",
    "age": 25,
    "isMale": true,
    "purchaseHistory" : [
        {
            "name": "cake",
            "price": 10.5
        },
        {
            "name": "iPad",
            "price": 699
        },
    ],
}
```

## yaml & toml
yaml 诞生于 2001 年。
yaml 一般用于配置文件，而不是像 json 那样在网络、内存、磁盘中传来传去。  
yaml 是一个可读性高，用来表达数据序列化的格式。YAML参考了其他多种语言，包括：C语言、Python、Perl，并从 xml、电子邮件的数据格式（RFC 2822）中获得灵感。

yaml 语法多且散，容忍性较高。
语法特点：以缩进为 block（块），字符串不必用引号引起来，yes/no 也被视为 boolean，

toml


## xml

xml 是诞生于 1998 年的一种标记语言和用于存储、传输和重构松散数据的文件格式。xml 一般用于**配置文件**（尤其是 java 生态），目前逐渐淡出**传输数据**的情景。  

html 是从 xml 衍生而来的，但二者并不等同，因此 xml 的语法主要参考 html，不再赘述。除了繁琐外，xml 的第一行用于规定文件基础信息。  
* .xml
```xml
<?xml version="1.0" encoding="UTF-8"?>
<user>
    <name>Tom</name>
    <age>25</age>
    <isMale>true</isMale>
    <purchaseHistory>
        <item>
            <name>cake</name>
            <price>10.5</price>
        </item>
        <item>
            <name>iPad</name>
            <price>699</price>
        </item>
    </purchaseHistory>
</user>
```

## csv

## sh

