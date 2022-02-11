
# CSS 基础语法

- CSS 使样式和结构分离，样式和结构不用杂糅着写，而是彼此分开：HTML就负责结构，CSS负责样式            

- HTML怎么结合，选择器就是一个纽带   
- 
## css 的书写位置        
- 内嵌式: 
- 外链式：link标签引入   
- 导入式：@import url(css/css.css)   
    - 使用导入式引入的样式表，不会等待css 文件加载完毕，而是会立即渲染HTML结构，所以页面会有几秒钟的“素面朝天”的时间
- 行内式：样式可以通过style 属性写在标签身上
    - 行内式牺牲了样式表的批量设置样式的能力，只能给一个标签设置样式，所以不常用     

**原子类**   

在做网页项目前，可以将所有的常用字号、文字颜色、行高、外边距、内边距等都设置为单独的类    

同一个标签可以同时属于多个类，类名用空格隔开。    

## CSS 选择器
#### 选择器类型
- 复合选择器   
- 后代选择器 使用空格表示后台.box p
- 交集选择器 
- 并集选择器      

选择器可以任意搭配，结合，从而复合选择器，我们必须能一目了然看出选择器代表的含义     

#### 伪类     
添加到选择器的描述性词语，指定要选择的元素的特殊状态，超级链接拥有4个特殊状态   
- a:link
- a:visited
- a:hover
- a:active    


**1、元素关系选择器**      
- 子选择器：div>p     

div 的子标签P   IE7开始兼容
- 相邻兄弟选择器   

>img+p  图片后面紧跟着的段落将被选中

- 通用兄弟选择器

>p~span p 元素后所有同层级span 元素         

**2、序号选择器**
- :first-child:第一个元素    IE7
- :last-child:最后一个子元素   IE9 
- :nth-child(3):第3个子元素       IE9
- :nth-of-type(3):第3个某类型子元素  IE9    
- :nth-last-child(3):倒数第3个子元素   IE9 
- :nth-last-of-type(3):倒数第三个某类型子元素 IE9        

**3、属性选择器**   

IE9开始兼容   
- empty:选择空标签
- focus:选择当前获得焦点的表单元素
- enabled:选择当前有效的表单元素
- disabled：选择当前无效的表单元素
- checked:选择当前已经勾选的单选按钮或复选框
- root:根据根元素，即<html>标签


# 文本与字体属性    
## 颜色的表示   

**1、十六进制表示法**
- 十六进制表示法是所有软件设计中都通用的颜色表示法，设计师给我们的设计图上标注的演的，通常都为十六进制表示
- 比如十六进制ff就是十进制的255，每种颜色分量都是0~255的数字   
- 如果颜色值是#aabbcc,可以简写为#abc     


**2、rgb表示法**
```
color:rgb(255,0,0);   
```
**3、rgba 表示法**     
颜色也可以使用rgba()表示法，最后一个参数表示透明度，介于0到1之间，0表示纯透明，1表示纯实心。
```
color:rgbs(255,0,0,.65)
```
注意：rgba 是从IE9开始兼容    


## 常用文本样式属性
**1、color 属性**    
- color 属性可设置文本内容的颜色
- color 属性主要可以用英语单词，十六进制，rgb(),rgba()等表示法
- 英语单词表示法，比如color:red;仅仅用于学习临时设置一下颜色，工作是基本不用这样的形式，因为追求精确   

    
**2、font-size 属性**   

用来设置字号，单位通常为px,除此之外还有em,rem等等单位。    
网页文字正文字号通常是16px,浏览器最小支持10px           
- font-weight 设置字体的粗细层度，通常就用normal和bold两个值       

|属性值|意义|    
|-|-|    
|font-weight:normal|正常粗细，与400等值|
|font-weight:bold|加粗，与700等值|   
|font-weight:lighter|更细，大多数中文字体不支持|    
|font-weight:bolder|大多数中文字体不支持|             

**font-style 属性**   

设置字体的倾斜    

|属性值|意义|
|-|-|    
|font-style:normal|取消倾斜，可以把天生倾斜的i、em等标签设置为不倾斜|
|font-style:italic|设置为倾斜字体|
|font-style:oblique|设置为倾斜字体（用常规字体模拟，不常用）|          

**text-decoration属性**    

用于设置文本的修饰线外观（下划线，删除线）   

|属性值|意义|     
|-|-|    
|text-decoration:none|没有修饰线|
|text-decoration:underline|下划线|   
|text-decoration:line-through|刪除线|


## 字体属性详解 
- font-family 设置字体     
    - 字体可以是列表形式，后面的字体是前面字体的备选字体。
    - 字体名称中有空格，必须用引号包裹
    - 英语字体放在最前面,网页中的中文符号在英文字体中没有，所有的字符会现在英文字体中去寻找
    - 这个问题字体也可以称呼它们的英文名字，中文字体有自己的对应的英文名   
    - 字体通常为用户在计算机中已经安装好的字体，所以一般来说设置为微软雅黑和宋体较多，设置成其他字体较少
    - 如何为设置为用户电脑中没有的字体呢，那就必须自己定义新字体，这就需要我们有字体文件用户加载网页的时候，会同时下载这些字体文件    
    - 字体文件根据操作系统和浏览器不同，有eot、woff2、woff、ttf、svg 文件格式，需要同时有这5中文件
    - 当我们拥有新的字体文件后，就可以用@font-face 定义字体，
    - 阿里巴巴普惠体，阿里巴巴提供了一套商用授权的普惠字体，使用阿里巴巴普惠字体也省去了下载字体的麻烦

## 段落和行相关属性
- text-indent 定义首行文本内容之前的缩进量

- line-height 属性用于定义行高
    - 单位可以是以px 为单位的数值
    - 也可以是没有单位的数值，表示字号的倍数，则是比较推荐的写法

**单行文本垂直居中**     
- 设置行高=盒子高度，即可实现单行文本垂直居中。 
```
    .box {
            height: 100px;
            width: 200px;
            margin: 100px 100px;
            background-color: blueviolet;
            text-decoration-color: white;
            line-height: 100px;
            text-align: center;
        }
    <div class="box">
        文本垂直居中/水平居中
    </div>
```

**font 属性合写**    

font 属性可以用来作为font-style,font-weight,font-size,line-height和font-family 属性的合写
```
font: 20px/1.5 Arial,"微软雅黑"    
font: italic bold 20px/1.5 Arial,"微软雅黑"
```


## 继承性   
文本相关的属性普遍具有继承性，只需要给祖先标签设置，即可在后代所有标签中设置。
- color
- font 开头的
- list 开头的
- text 开头的
- line 开头的

因为文字相关的属性都具有继承性，所以通常会设置body标签的字号、颜色、行高等，这样就能当做整个网页的默认样式了。    


## 其他   

**就近原则**   
在继承的情况下，选择器权重计算失效，而是“就近原则”   
在元素没有直接选择的情况下，通过继承得到的属性，优先就近原则，其次再选择选择器权重选择属性。


# 盒模型       
所有的HTML 元素都可以看做是一个盒子，盒子分为四个部分的内容,由内而外分为：
- 内容区域 Content: 盒子的内容，显示文字和图像
- 内边距 Padding: 内容周围的区域，为透明的
- 边框 Border: 盒子的边框，在内边距和外边距中间       
- 外边距 Margin: 边框外的边距，       

当我们设置元素的宽度和高度时，其实只是设置元素的内容区域的宽高，除此之外，还必须添加内外边距以及边框的距离，由此可以得到以下结论：
- 元素的总宽度计算公式   
>总元素宽度= 宽度+左内边距+右内边距+左边框+右边框  

- 元素的总高度计算公式
> 总元素高度= 宽度+左内边距+右内边距+左边框+右边框
    
一旦为页面设置了恰当的 DTD，大多数浏览器都会按照上面的图示来呈现内容。然而 IE 5 和 6 的呈现却是不正确的。根据 W3C 的规范，元素内容占据的空间是由 width 属性设置的，而内容周围的 padding 和 border 值是另外计算的。不幸的是，IE5.X 和 6 在怪异模式中使用自己的非标准模型。这些浏览器的 width 属性不是内容的宽度，而是内容、内边距和边框的宽度的总和。

虽然有方法解决这个问题。但是目前最好的解决方案是回避这个问题。也就是，不要给元素添加具有指定宽度的内边距，而是尝试将内边距或外边距添加到元素的父元素和子元素。

## 边框       
**边框样式 border-style**    
    
    定义边框的样式     
    
**边框宽度 border-width**      
    
    为边框指定宽度有两种方法，
    1、可以指定像素值，如2px(px,pt,em,cm)
    2、使用三个关键字:thick,medium,thin    

**边框颜色border-color**    
    
    颜色的三种表示方法：
    1、英文名称
    2、RGB
    3、十六进制    
    
**边框-单独设置各边**    
    
    CSS 可以设置不同侧面的边框为不同的样式
    
如下所示，依次为顶部边框，右侧边框，底部边框，左侧边框
```
    border-top-style:dotted;
    border-right-style:solid;
    border-bottom-style:dotted;
    border-left-style:solid;
```
除此之外，也可以简写如下：
```
<!--依次为上右下左-->
border-style:dotted solid double dashed;
<!--依次为上，左右，下-->
border-style:dotted solid double;
<!--依次为上下，左右-->
border-style:dotted solid;
```      

**边框-简写属性**       

    类似于之前font 属性的简写，同样，边框也有很多属性，不必一个一个写清楚，可以用border 属性简写
    
如下所示,顺序如下：
- border-width
- border-style (required)
- border-color   
 
```
border:5px solid red;
```    
## 外边距 margin    
margin 是盒子的外边距，是盒子和其他盒子之间的距离，是有四个方向的：
- margin-top
- margin-right
- margin-bottom
- margin-left    

#### margin 的塌陷    
竖直方向的margin 有塌陷现象，小的margin会塌陷到大的margin中，从而margin不叠加，以相对来说大的margin为准：
```
<style>
        .box1 {
            width: 200px;
            height: 200px;
            background-color: orange;
            margin-bottom: 20px;
        }
    
        .box2 {
            width: 200px;
            height: 200px;
            background-color: blue;
            margin-top: 30px;
        }
    </style>
<body>
    <div class="box1"></div>
    <div class="box2"></div>
</body>
```
box1 底部的margin 是20px,box2 顶部的margin是30px,所以box1和box2之间的margin 是30px,以相对大的那个值为准。   
水平方向的margin 不塌陷。    

#### 默认的margin     
一些元素（如body，ul,p）都有默认的margin,在开始制作网页的时候，要将他们清除，以免对我们网页的设计精度有影响。       

- 将盒子左右两边的margin都设置为auto,盒子将水平居中。   
- 文本居中是text-algn:center,和盒子在水平居中是两个概念
- 盒子的垂直居中需要使用绝对定位来实现的          

## 内边距 padding 
padding 是四个方向的，分别用小属性设置
- padding-top
- padding-right
- padding-bottom
- padding-left   
```
    .box1 {
        width: 200px;
        height: 200px;
        padding-top: 10px;
        padding-right: 30px;
        padding-bottom: 50px;
        padding-left: 70px;
        background-color: orange;
    }
```

#### padding 的各种写法
**1、padding 的四数值写法**    
padding 属性用四个数值以空格隔开进行设置，分别表示上，右，下，左的padding 

```
    .box1 {
        width: 200px;
        height: 200px;
        padding: 10px 30px 50px 70px;
        background-color: orange;
    }
```     
**2、padding 的三数值写法**      
padding 如果用三个数值以空格隔开进行设置，分别表示上，左右，下的padding     
```
    .box1 {
        width: 200px;
        height: 200px;
        padding:10px 20px 30px;
        background-color: orange;
    }
```
**3、padding 的二数值写法**      
padding 如果用三个数值以空格隔开进行设置，分别表示上下，左右的padding     
```
    .box1 {
        width: 200px;
        height: 200px;
        padding:10px 20px;
        background-color: orange;
    }
```    

## 其它
#### box-sizing 属性   
- 将合盒子添加了```box-sizing:border-box```之后，盒子的width,height 数字就表示盒子实际占有的宽高（不含margin），即padding、border变为内缩的，不在外扩      
- ```box-sizing```属性大量应用于移动网页制作中，因为它结合百分比布局，弹性布局等非常好用，在PC 页面开发中使用较少
- 兼容到IE9     

```
        .boxsizing {
            box-sizing: border-box;
            width: 200px;
            height: 200px;
            border: 10px solid #000;
            padding: 10px;
        }
    
    <!-- box-sizing 属性 -->
    <div class="boxsizing"></div>
```       
如上所示的一个盒子，设置了box-sizing 属性后，实际的宽高从之前的240px\*240px变为200px\*200px了，padding 和border 向内缩进。   

这个属性分别有三个值：
- content-box    

宽度和高度分别对应到元素的内容框
- border-box     

宽度和高度对应的就是元素的宽高总和，即content+padding+border总和为width和height。  
- inherit 
表示从父元素继承box-sizing 值。   

有一个div 和 button 元素，除了背景色之外，其他样式都相同，在chrom 浏览器上，二者的显示效果却有差异。   
```
链接地址：https://class.imooc.com/lesson/jobdetail?mid=44022#
```   

#### display 属性   

##### 说在前面
**行内元素和块级元素**    
|display属性类型|是否能并排显示|是否能设置宽高|当不设置width属性时|举例|       
|-|-|-|-|-|-|     
|块级元素|否|是|width自动撑满|div,section,header,h系列，li,ul等|     
|行内元素|是|否|width自动收缩|a,span,em,b,u,i等|                       

**行内块**      

img 和表单元素是特殊的行内块，它们既能设置宽度高度，也能够并排显示   


**行内元素和块级元素的相互转换**       
- 使用```display:block;``` 可将元素转为块级元素   
- 使用```display:inline;```即可将元素转为行内元素，将元素转为行内元素的应用不多见   
- 使用```display:inline-block;```即可将元素转为行内块         

#### 元素的隐藏    
- 使用```display:none;```     
- 可以将元素隐藏，元素将彻底放弃位置，如同没有它的标签一样。
使用```visibility:hidden;```，也可以将元素隐藏，但是元素不放弃自己的位置        


## 总结   
1、什么是盒模型？
- 深入理解 width 和 height 属性   
- box-sizing 属性的作用
- 盒子的实际尺寸和空间尺寸   
- margin 的塌陷现象      

2、盒模型的计算
- 父子盒模型的计算    
- 行内元素和块级元素的重点     
- 元素的隐藏   