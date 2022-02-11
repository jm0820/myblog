## 页面结构
一个标准的HTML页面如下：
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>

</body>
</html>
```     
#### 一、文档声明头   
标准的HTML页面，第一行是以```<!DOCTYPE html>```开头的的语句，这就是文档声明头，即DocType Declaration，简称DTD。DTD可以告知浏览器使用哪种HTML或者XHTML规范。     
#### 二、页面语言      
第二行标签，用来指定页面的语言类型   
```
<html lang="en">
```
- HTML 标签:页面中最大的标签，称之为根标签     
- lang:指定页面的语言类型，常见语言类型有两种
    - en：定义页面语言为英语   
    - zh-CN:定义页面语言为中文     

#### 头标签   
头标签里面一般表示的是页面的配置，内部的常见标签如下：
|标签|说明|
|-|-|
|title|整个网页的标题|
|base|为页面上所有标签规定默认地址或者默认标签|     
|meta|提供有关页面的基本信息|
|link|定义文档与外部资源的关系|          

##### meta 标签      
meta 表示“元”，“元”配置，就是表示基本的配置项目     
常见的几种meta标签如下：       
1、字符集    
```
<meta charset="UTF-8">
```      
网页的编码方式，计算机要处理各种字符集文字，需要进行字符编码，以便计算机能够识别和存储各种文字。    

2、视口（viewport）    
```
 <meta name="viewport" content="width=device-width, initial-scale=1.0">
```
width=device-width:表示视口宽度等于屏幕宽度     

3、页面描述（Description）           
```
<meta name="Description" content="Description 搜索结果">
```
只要设置了Description页面描述,那么百度搜索结果就能显示这个语句，这个技术叫做SEO（search engine optimization,搜索引擎优化）

4、关键字Keywords
```
<meta name="Keywords" content="关键字">
```     
这些关键词，就是告诉搜索引擎，这个网页是干嘛的，能够提高搜索命中率。让别人能够找到你，搜索到你。   
##### title 标签    
```
<title>标题-HTML学习</title>
```
网页的标题     
##### base 标签
```
<base href="/">
```
base 标签用于指定基础的路径，指定之后，所有的a 标签都是以这个路径为准。    

##### link 标签
```
<link rel="stylesheet" type="text/css" href="theme.css">
```
link 标签链接了外部的标签。                      

#### 三、body 标签    
所有的网页内容都写在body里面。            

## 语义   
HTML的语义化基本上是围绕着几个主要的标签展开，类似列表，标题等等。   
示例       
|标签|效果|语义|
|-|-|-|    
|```<h1>```|加黑加粗|一级标题|
|```<li>```|多个小圆点|列表|      

#### 为什么要语义化
- 为了在没有CSS的情况下，页面也能呈现出很好地内容结构、代码结构:为了裸奔时好看。
- 用户体验：例如title、alt用于解释名词或解释图片信息、label标签的活用。
- 有利于SEO：和搜索引擎建立良好沟通，有助于爬虫抓取更多的有效信息：爬虫依赖于标签来确定上下文和各个关键字的权重。
- 方便其他设备解析（如屏幕阅读器、盲人阅读器、移动设备）以意义的方式来渲染网页。
- 便于团队开发和维护，语义化更具可读性，是下一步吧网页的重要动向，遵循W3C标准的团队都遵循这个标准，可以减少差异化。
#### 写HTML 代码要注意些什么
- 尽可能少的使用无语义的标签div和span。
- 在语义不明显时，既可以使用div或者p时，尽量用p,因为p在默认情况下有上下间距，对兼容特殊终端有利。
- 不要使用纯样式标签，如：b、font、u等，改用css设置。
需要强调的文本，可以包含在strong或者em标签中（浏览器预设样式，能用CSS指定就不用他们），strong默认样式是加粗（不要用b），em是斜体（不用i）。
- 使用表格时，标题要用caption，表头用thead，主体部分用tbody包围，尾部用tfoot包围。表头和一般单元格要区分开，表头用th，单元格用td，
表单域要用fieldset标签包起来，并用legend标签说明表单的用途。
- 每个input标签对应的说明文本都需要使用label标签，并且通过为input设置id属性，在lable标签中设置for=someld来让说明文本和相对应的input关联起来。



