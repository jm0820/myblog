## 表格的基本使用
- ```<table>```: 标签定义了HTML表格   
- ```<tr>```: 定义表格行     
- ```<th>```: 定义表头    
- ```<td>```: 定义表格单元    
示例
```
<table border="1" width="500">
         <caption>表格的标题</caption>
         <tr>
             <!-- th 表格的标题小格 -->
             <th>第一季度</th>
             <th>第二季度</th>
             <th>第三季度</th>
             <th>第四季度</th>
         </tr>
         <tr>
             <td>A</td>
             <td>B</td> 
             <td>C</td>
             <td>D</td>
         </tr>
         <tr>
            <td>E</td>
            <td>F</td>
            <td>G</td>
            <td>H</td> -->
        </tr>
        <tr>
            <td>I</td>
            <td>J</td>
            <td>K</td>
            <td>L</td>
        </tr>
        <tr>
            <td>M</td>
            <td>N</td>
            <td>O</td>
            <td>P</td> 
        </tr>
     </table>
```
效果  
<table border="1" width="500">
         <caption>表格的标题</caption>
         <tr>
             <!-- th 表格的标题小格 -->
             <th>第一季度</th>
             <th>第二季度</th>
             <th>第三季度</th>
             <th>第四季度</th>
         </tr>
         <tr>
             <td>A</td>
             <td>B</td> 
             <td>C</td>
             <td>D</td>
         </tr>
         <tr>
            <td>E</td>
            <td>F</td>
            <td>G</td>
            <td>H</td> -->
        </tr>
        <tr>
            <td>I</td>
            <td>J</td>
            <td>K</td>
            <td>L</td>
        </tr>
        <tr>
            <td>M</td>
            <td>N</td>
            <td>O</td>
            <td>P</td> 
        </tr>
     </table>
- caption:是表格的标题，常常作为 table 的子标题出现   

## 复杂的表格   
#### 表格的属性       

- ```colspan```:设置td或者th的列跨度     
- ```rowspan```: 用来设置td 或者th的行跨度  
- ```<thead>```：标签定义表头    
- ```<tbody>```：标签定义表核心内容    
- ```<tfoot>```：标签定义表脚，通常是汇总行    
- ```cellpadding```： 属性定义了表格单元的内容和边框之间的空间，已经废弃，使用CSS替代它
- ```cellspacing```：属性，使用百分比或像素，定义了两个单元格之间空间的大小，已经废弃，使用CSS替代它

#### 单元格的合并
1. 合并列      
使用 colspan 属性设置列跨度（记得在合并列跨度的时候减少当前行的列数）     
合并第一行的一二列，也就是A,B单元格(在给A单元格设置列跨度的时候，记得删除B单元格)
```
<table border="1" width="500">
         <caption>表格的标题</caption>
         <tr>
             <!-- th 表格的标题小格 -->
             <th>第一季度</th>
             <th>第二季度</th>
             <th>第三季度</th>
             <th>第四季度</th>
         </tr>
         <tr>
             <td colspan='2'>A</td>
             <td>C</td>
             <td>D</td>
         </tr>
         <tr>
            <td>E</td>
            <td>F</td>
            <td>G</td>
            <td>H</td> -->
        </tr>
        <tr>
            <td>I</td>
            <td>J</td>
            <td>K</td>
            <td>L</td>
        </tr>
        <tr>
            <td>M</td>
            <td>N</td>
            <td>O</td>
            <td>P</td> 
        </tr>
     </table>
```
<table border="1" width="500">
         <caption>表格的标题</caption>
         <tr>
             <!-- th 表格的标题小格 -->
             <th>第一季度</th>
             <th>第二季度</th>
             <th>第三季度</th>
             <th>第四季度</th>
         </tr>
         <tr>
             <td colspan='2'>A</td>
             <td>C</td>
             <td>D</td>
         </tr>
         <tr>
            <td>E</td>
            <td>F</td>
            <td>G</td>
            <td>H</td> -->
        </tr>
        <tr>
            <td>I</td>
            <td>J</td>
            <td>K</td>
            <td>L</td>
        </tr>
        <tr>
            <td>M</td>
            <td>N</td>
            <td>O</td>
            <td>P</td> 
        </tr>
     </table>
     
2. 合并行   
使用 rowspan 属性设置行跨度（记得在合并列跨度的时候减少当前列的行数）     
合并第三列的三四列，也就是K,O单元格(在给K单元格设置行跨度的时候，记得删除O单元格)
```
<table border="1" width="500">
         <caption>表格的标题</caption>
         <tr>
             <!-- th 表格的标题小格 -->
             <th>第一季度</th>
             <th>第二季度</th>
             <th>第三季度</th>
             <th>第四季度</th>
         </tr>
         <tr>
             <td>A</td>
             <td>B</td> 
             <td>C</td>
             <td>D</td>
         </tr>
         <tr>
            <td>E</td>
            <td>F</td>
            <td>G</td>
            <td>H</td> -->
        </tr>
        <tr>
            <td>I</td>
            <td>J</td>
            <td rowspan='2'>K</td>
            <td>L</td>
        </tr>
        <tr>
            <td>M</td>
            <td>N</td>
            <!--<td>O</td>-->
            <td>P</td> 
        </tr>
     </table>
```
效果
<table border="1" width="500">
         <caption>表格的标题</caption>
         <tr>
             <!-- th 表格的标题小格 -->
             <th>第一季度</th>
             <th>第二季度</th>
             <th>第三季度</th>
             <th>第四季度</th>
         </tr>
         <tr>
             <td>A</td>
             <td>B</td> 
             <td>C</td>
             <td>D</td>
         </tr>
         <tr>
            <td>E</td>
            <td>F</td>
            <td>G</td>
            <td>H</td> -->
        </tr>
        <tr>
            <td>I</td>
            <td>J</td>
            <td rowspan='2'>K</td>
            <td>L</td>
        </tr>
        <tr>
            <td>M</td>
            <td>N</td>
            <!--<td>O</td>-->
            <td>P</td> 
        </tr>
     </table>   
     
#### 表格的其它特性        

- ```<thead>```：标签定义表头    
- ```<tbody>```：标签定义表核心内容    
- ```<tfoot>```：标签定义表脚，通常是汇总行     
```
<table border="1">
  <thead>
    <tr>
      <th>Month</th>
      <th>Savings</th>
    </tr>
  </thead>
  <tfoot>
    <tr>
      <td>Sum</td>
      <td>$180</td>
    </tr>
  </tfoot>
  <tbody>
    <tr>
      <td>January</td>
      <td>$100</td>
    </tr>
    <tr>
      <td>February</td>
      <td>$80</td>
    </tr>
  </tbody>
</table>
```
thead, tbody, 和 tfoot 元素默认不会影响表格的布局。不过，您可以使用 CSS 来为这些元素定义样式，从而改变表格的外观。
<table border="1">
  <thead>
    <tr>
      <th>Month</th>
      <th>Savings</th>
    </tr>
  </thead>
  <tfoot>
    <tr>
      <td>Sum</td>
      <td>$180</td>
    </tr>
  </tfoot>
  <tbody>
    <tr>
      <td>January</td>
      <td>$100</td>
    </tr>
    <tr>
      <td>February</td>
      <td>$80</td>
    </tr>
  </tbody>
</table>
