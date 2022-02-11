## 作用
采集用户的信息，如，用户名，密码等.

## 表达的创建   
表单标签
以<form> 元素开始

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>表单</title>
</head>
<body>
     <form action="" method="POST">
           
     </form>
</body>
</html>    
```   
被 form 标签包裹起来的部分就是表单的部分，所有的输入框就是包在form标签内的。

## 表单类型
表单标签
```
<input type="">
```
type 是指表单类型,表单类型一般来说有以下几种类型:单行文本框，单选按钮，复选框，密码框，下拉菜单，多行文本框，按钮。     
#### 单行文本框
```
请输入你的姓名：<input type="text">
```
使用type 属性被设置为text的input元素可以创建单行文本框，它是一个单标
签。      

**属性**
- ```value```：已经写好的值，或者说是表单被提交是本文本框提供的值。
- ```placeholder```：提示文字，将以浅色文字写在文本框中，并不是文本框的值。
- ```disable```：表示元素不能与用户交互，即“锁死”。       
```
<p>
    报考院校：<input type="text" value="清华大学" disabled>
</p>
<p>
    毕业学校：<input type="text" placeholder="请输入真实的毕业学校哦！">
</p>
```
        
          
#### 单选按钮     
```
    <input type="radio">
```
使用type 属性值被设为radio的<input> 元素可以创建单选按钮      

**属性**     
- ```name```：互斥的单选按钮应该设置它们的name 属性为相同值
- ```checked```：表示默认被选中
- ```label```：用来将文字和单选按钮进行绑定，用户单击文字的时候也是为点击了单选按钮；在HTML4时代，label标签是通过for属性和单选按钮的id属性进行绑定的
```
<p>
    性别：
    <label>
        <input type="radio" name="sex" value="男" checked>男
    </label>    
    <label>
        <input type="radio" name="sex" value="女">女
    </label>
</p>     

<p>
    血型：
    <input type="radio" name="bloodtype" value="A" id="A" checked>
    <label for="A">A</label>
    <input type="radio" name="bloodtype" value="B" id="B">
    <label for="B">B</label>
    <input type="radio" name="bloodtype" value="AB" id="AB">
    <label for="AB">AB</label>
    <input type="radio" name="bloodtype" value="O" id="O">
    <label for="O">O</label>
</p>
```

#### 复选框    
```
    <input type="checkbox">
```
使用type 属性值被设置为checkbox的<input> 元素可以创建复选框。      
同组复选框应该设置它们的name为相同值。     
复选框要有value属性值，向服务器提交的就是value值。 
```
<p>
    爱好：
    <label>
        <input type="checkbox" name="hobby" value="足球">足球
    </label>
    <label>
        <input type="checkbox" name="hobby" value="篮球">篮球
    </label>
    <label>
        <input type="checkbox" name="hobby" value="羽毛球">羽毛球
    </label>
    <label>
        <input type="checkbox" name="hobby" value="乒乓球">乒乓球
    </label>
    <label>
        <input type="checkbox" name="hobby" value="网球">网球
    </label>
</p>     
```
#### 密码框     
使用type属性值被设置为password 的<input> 元素可以创建密码框 
```
<p>
  请输入密码：<input type="password" name="" id="">
</p>
```
密码框输入内容是显示为```*******```
#### 下拉菜单      
```<select>``` 表示下拉菜单，```<option>```是它内部的选项     

```
选择你喜欢的支付方式：
<select name="" id="">
    <option value="alipay">支付宝</option>
    <option value="wx">微信</option>
    <option value="bank">网银</option>
</select>
```

#### 多行文本框     
一般用来书写比较多的文字，比如备注等……
```
<p>
    留言：
    <textarea name="" id="" cols="30" rows="10"></textarea>
</p>
```   

#### 按钮
表单中常见的三种按钮，它们也都是input标签，type属性值不同。     
- ```button```：普通按钮，可以自行添加事件
- ```submit```：提交按钮，提交表单
- ```reset```：重置按钮，清空输入框的内容    

```
<p>
    <button>我是一个普通按钮</button>
</p>
<p>
    <input type="button" value="我是一个普通按钮">
</p>
<p>
    <input type="submit" value="提交">
</p>
<p>
    <input type="reset" value="重置">
</p>   
```


## 其它
更丰富的input 种:  
- color：颜色选择控件
```
 <p>
            颜色选择控件：<input type="color">
        </p>
```

- date,time；日期，时间选择控件
```
<p>
         日期选择：<input type="date">
        </p>
        <p>
            时间选择：<input type="time">
        </p>
```
- email:电子邮件输入控件   
```
<p>
    文件选择:<input type="file" >
</p>
```
- file:文件选择控件
```
<p>
    文件选择:<input type="file" >
</p>
```
- number:数字输入控件
```
<p>
    数字输入:<input type="number" min="10" max="20">
</p>
```
- range:拖拽条
```
<p>
    拖拽条:<input type="range" min="10" max="20">
</p>
```
- search:搜索框
```
<p>
    搜索:<input type="search">
</p>
```
- url:  网址输入控件
```
<p>
    网址:<input type="url">
</p>
```
- datalist       
<datalist> 控件可以为输入框提供一些备选项，当用户输入的内容与被选项文字相同时，将会显示智能感应
```
 <input type="text" list="province-list">
 <datalist id="province-list">
    <option value="陕西">
    <option value="北京">
    <option value="湖南">
    <option value="广东">
    <option value="山东">
    <option value="山西">
    <option value="江苏">
 </datalist>
```
#### label 标签
将文字和表单控件进行绑定，用户点击文字时也视为点击了表单控件
- ```<label>``` 标签为 input 元素定义标注（标记）
- label 元素不会向用户呈现任何特殊效果。不过，它为鼠标用户改进了可用性。如果您在 label 元素内点击文本，就会触发此控件。就是说，当用户选择该标签时，浏览器就会自动将焦点转到和标签相关的表单控件上。
- <label> 标签的 for 属性应当与相关元素的 id 属性相同。
```
<form>
  <label for="male">Male</label>
  <input type="radio" name="sex" id="male" />
  <br />
  <label for="female">Female</label>
  <input type="radio" name="sex" id="female" />
</form>
```
- 点击Male 鼠标就会将焦点转到和其绑定的的id为male 的元素上(包裹Male 的label标签的for 属性是male,第一个输入框的ID和for 属性的值相同，即此label标签和input 标签绑定起来)
- 点击female 鼠标就会将焦点转到和其绑定的的id为female 的元素上（同上）   

#### 表单校验         
required 属性规定必需在提交之前填写输入字段，为必填项。   
required 属性适用于以下 <input> 类型：text, search, url, telephone, email, password, date pickers, number, checkbox, radio 以及 file。

```
<input required="required">
```