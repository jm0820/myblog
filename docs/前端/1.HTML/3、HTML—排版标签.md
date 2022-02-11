## 标题标签
```<h1>```至```<h6>``` 是标题标签，由1-6标题逐渐降级。  

属性
- align 对齐方式
    - left：居左对齐
    - center：居中对齐
    - right：居右对齐      

示例：
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <h1>一级标题</h1>
    <h2>二级标题</h2>
    <h3>三级标题</h3>
    <h4>四级标题</h4>
    <h5>五级标题</h5>
    <h6>六级标题</h6>
</body>
</html>
```
效果:
<h1>一级标题</h1>
<h2>二级标题</h2>
<h3>三级标题</h3>
<h4>四级标题</h4>
<h5>五级标题</h5>
<h6>六级标题</h6>     


## 段落标签   
HTML的标签被分为```文本级标签```和```容器级标签```,
- 文本级标签只能存放文字、图片、表单等元素（a 标签里不能放a和input）。
```
p,span,a,b,i,u,em
```
- 容器级标签可以放任何东西。
```
div,h系列,li,dt,dd
```

#### 1、水平线标签   
```
<hr />
```
在视觉上用水平线将文档分隔开来

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
    段落1
    <hr />   
    段落2
</body>
</html>
```
**效果**

段落1
<hr />   
段落2            


**属性**   
|属性名|说明|值|
|-|-|-|     
|align|设置线条放置位置|left,right,center|
|size|设置线条粗细，单位为像素|默认值为2|
|width|设置线条长度|绝对值（500）或者相对值（70%）|
|color|线条颜色||
|noshade|不要阴影，设定线条为平面显示|没有这个属性则表明线条具阴影或立体|               


#### 换行标签  
```
<br />
```
给某段文本强制换行。   

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
    落霞与孤鹜齐飞<br />秋水共长天一色
</body>
</html>
``` 

**效果**     
落霞与孤鹜齐飞<br />秋水共长天一色

>因为HTML 的空白折叠现象(HTML 的文字之间，如果有空格、换行或Tab都将被折叠成一个空格显示),我们只能使用```<br />``` 来强制换行。                                   


#### div 与 span 
- div:把标签的内容分隔为独立的区块,单独占据一行
- span:把标签的内容分隔为独立的区块,不换行     

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
    <div>div 块1</div>
    <div>div 块2</div>
    <span>span 块1</span>
    <span>span 块2</span>
</body>
</html>
```   
**效果**                 

<div>div 块1</div>
<div>div 块2</div>
<span>span 块1</span>
<span>span 块2</span>

<hr />   

**区别**    
|标签|是否换行|语义区别|
|-|-|-|   
|div|||
|span|||
1. div 换行，span 不换行
2. div 是一个容器级标签，里面什么都可以放。span 是一个文本级标签，里面只能放文字、图片、表单元素。    

#### 内容居中标签
```
<center></center>
```
center 标签里面的内容，都会居于浏览器的中间。    
到了HTML5里面，center标签不建议使用，建议使用css布局来实现。

#### 预定义（格式化）标签
```
<pre></pre>   
```         
保留标签内部的所有空白字符（空格，换行符），原封不动的输出结果（告诉浏览器不要忽略空格和空行）           

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
    <pre>
        落霞与孤鹜齐飞
        秋水共长天一色
    </pre>
</body>
</html>
```
**效果**   
<pre>
    落霞与孤鹜齐飞
    秋水共长天一色
</pre>
   

## HTML 中元素的嵌套关系
1. 块元素可以包含内联元素或者块元素，但内联元素不能包含块元素，只能包含其它内联元素
2. 块级元素不能放在P里面   
3. 有几个特殊的块元素只能包含内联元素，不能包含块元素。  
> h1,h2,h3,h4,h5,h6,p,dt
4. 块元素与块元素并列，内联元素与内联元素并列             

