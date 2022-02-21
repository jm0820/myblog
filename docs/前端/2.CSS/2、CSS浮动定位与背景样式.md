# 浮动  
作用：用来实现并排     
- 浮动使用要点：要浮动，并排的盒子都要设置浮动    
- 父盒子要有足够的宽度，否则盒子会掉下去。           
- 浮动的顺序贴靠特性   

示例

```html

<div class="box">
        <div class="c1"></div>
        <div class="c2"></div>
        <div class="c3"></div>
</div>      
```

```css
 .box {
            width: 600px;
            height: 200px;
            border: 1px solid #000;
        }    

        .box .c1 {
            width: 200px;
            height: 200px;
            background-color: orange;  
            float: left;
        }
        .box .c2{
            width: 200px;
            height: 200px;
            background-color: green;
            float: left;
        }
        .box .c3{
            width: 200px;
            height: 200px;
            background-color: blue;
            float: left;
        }
```

![image-20220221200553972](C:\Users\jiamengwang\AppData\Roaming\Typora\typora-user-images\image-20220221200553972.png)

子盒子会按照顺序进行贴靠，如果没有足够控件，则会寻找前一个兄弟元素         

示例

```html
  <div class="box2">
        <div class="c1"></div>
        <div class="c2"></div>
        <div class="c3"></div>
    </div>
```

```css
   .box2 .c1{
            width: 150px;
            height: 100px;
            background-color: orange;
            float: left;
        }

        .box2 .c2{
            width: 100px;
            height: 50px;
            background-color: green;
            float: left;
        }
        .box2 .c3{
            width: 100px;
            height: 50px;
            background-color: blue;
            float: left;
        }
```

![image-20220221200647986](C:\Users\jiamengwang\AppData\Roaming\Typora\typora-user-images\image-20220221200647986.png)

- 浮动的元素一定能设置宽高    

浮动的元素不再区分块级元素，行内元素，已经脱离了标准文档流，一律能够设置宽度和高度，即使它是span或者a 标签等。    

示例

```html
	<span>1</span>
    <span>2</span>
    <span>3</span>
    <span>4</span>
```

```css
  span {
            float: left;
            width: 100px;
            height: 30px;
            background-color: aqua;
            margin-right: 10px;
        }
```

![image-20220221201044987](C:\Users\jiamengwang\AppData\Roaming\Typora\typora-user-images\image-20220221201044987.png)

右浮动     
```float:right```     
右浮动是按照书写顺序去依次贴靠右边，比如书写顺序是123，三个块都设置右浮动，显示依次为321。       

```html
<div class="box3">
         <div class="c1">1</div>
        <div class="c2">2</div>
        <div class="c3">3</div>
    </div>
```

```css
 .box3 {
            width: 600px;
            height: 200px;
            border: 1px solid #000;
        }

        .box3 .c1{
            width: 200px;
            height: 200px;
            background-color: orange;
            float: right;
        }

        .box3 .c2{
            width: 200px;
            height: 200px;
            background-color: green;
            float: right;
        }
        .box3 .c3{
            width: 200px;
            height: 200px;
            background-color: blue;
            float: right;
        }
```

![image-20220221201542828](C:\Users\jiamengwang\AppData\Roaming\Typora\typora-user-images\image-20220221201542828.png)

BFC 规范    
BFC（Box Formatting Context 块级格式化上下文）是页面上的一个隔离的独立容器，容器里面的子元素不会影响到外面的元素，反之亦然。    

一个现象   
- 一个盒子不设置height，当内容子元素都浮动时，无法撑起自身.   
- 这个盒子没有形成 BFC.     

如何创建 BFC     
- float 不是none  
- 设置定位，定位一定要是绝对定位或者固定定位.     
- display 的值是inline-block flex 或者 inline-flex.
- overflow:hidden      



# 定位    

## 相对定位

## 绝对定位    

