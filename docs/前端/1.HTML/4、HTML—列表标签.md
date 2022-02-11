列表标签分为三种，有序列表，无需列表和自定义列表    
- 无序列表
- 有序列表
- 自定义列表    

## 无序列表
```<ul>```：unorder list，表示无序列表   
```<li>```:list item，列表项     

**示例**
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <ul >
        <li>牛奶</li>
            <li>
            咖啡
            <p>不要买黑咖啡</p>
        </li>
        <li>水果</li>
    </ul>
</body>
</html>
```
**效果**   
<ul >
    <li>牛奶</li>
    <li>
        咖啡
        <p>不要买黑咖啡</p>
    </li>
    <li>水果</li>
</ul>

**属性**       

无序列表默认前面是一个小圆点，属性type 设置了无序列表的图标，type 的值为下面几种，默认为disc(实心圆),
- circle：空心圆
- square：实心方块
- disc：实心圆     
```
 <ul type="circle">
        <li>牛奶</li>
            <li>
            咖啡
            <p>不要买黑咖啡</p>
        </li>
        <li>水果</li>
    </ul>
```
 <ul type="circle">
        <li>牛奶</li>
            <li>
            咖啡
            <p>不要买黑咖啡</p>
        </li>
        <li>水果</li>
    </ul>

**其他**

1. ul,li 表示列表的语义，不是每一项前面加个小圆点
2. 在style中设置： list-style-type:none 就把每个li前面的圆点去掉了。 

## 有序列表
```<ol>```：order list，表示无序列表   
```<li>```:list item，列表项     
**示例**
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <ol>
        <li>有序列表1</li>
        <li>有序列表2</li>
        <li>有序列表3</li>
        <li>有序列表4</li>
    </ol> 
</body>
```

**效果**   
<ol>
    <li>有序列表1</li>
    <li>有序列表2</li>
    <li>有序列表3</li>
    <li>有序列表4</li>
</ol>                    

**属性**     

默认用1,2,3表示项目列表,属性type用来设置项目列表。              
- a: 以a,b,c 表示
- A: 以A,B,C 表示
- I: 以I II III 表示
- i：以i ii iii 表示
start:编号开始的数值，默认1开始，可以省略          

**示例**
```
<ol type="A" start="2">
    <li>有序列表1</li>
    <li>有序列表2</li>
    <li>有序列表3</li>
    <li>有序列表4</li>
</ol>        
```
<ol type="A" start="2">
    <li>有序列表1</li>
    <li>有序列表2</li>
    <li>有序列表3</li>
    <li>有序列表4</li>
</ol>    

## 自定义列表
- 自定义列表不仅仅是一列项目，而是项目及其注释的组合。
- 自定义列表以```dl```标签开始。每个自定义列表项以```dt``` 开始。每个自定义列表项的定义以```dd```开始。

**示例**      
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
    <body>
        <dl>
            <dt>HTML</dt>
            <dd>超文本标记语言</dd>
            <dt>CSS</dt>
            <dd>层叠式样式表</dd>
            <dt>javaScript</dt>
            <dd>JS脚本程序</dd>
        </dl>
    </body>
</html>
```   

**效果**
<dl>
    <dt>HTML</dt>
    <dd>超文本标记语言</dd>
    <dt>CSS</dt>
    <dd>层叠式样式表</dd>
    <dt>javaScript</dt>
    <dd>JS脚本程序</dd>
</dl>   


## 总结
|标签|描述|   
|-|-|
|ul|定义无序列表|
|ol|定义有序列表|
|li|定义列表项|
|dl|自定义列表|
|dt|自定义列表项目|
|dd|自定义列表项描述|         
