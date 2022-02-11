**EL简介**  
EL（Express Language）   

**一、EL语法**
EL都是以"${"开始 "}"结尾的  

```
    ${expression}
```    
**二、EL变量**   
```${username}``` 表示取出某一范围中名称为username的变量，因为我们并没有指定哪一个范围的username,所以它的默认值会先从Page 范围找，假如找不到，再依序到Request、Session、Application范围,假如途中找到username,就直接回传,不再继续找下去,但是假如全部的范围都没有找到时，就回传null.

如果域对象里面有特殊字符,那么使用[]更方便一点.
```${username.My-name}```就应该改为```${username["My-name"]}```

- EL 可以进行自动的类型转换.    

**三、EL的隐含对象**

**与范围有关的隐含对象:** application、sessionScope、requestScope、pageScope
在EL中，这四个隐含对象只能用来取得范围属性值，即```getAttribute(String name)```,却不能取得其它相关信息.
取得Session 中的username值：
```
session.getAttribute("username")   
```   
使用EL表达式：
```
${sessionScope.username}
```   

**与输入有关的隐含对象:** param、paramValues
- ```${param}```: 表示返回请求参数中单个字符串的值.
- ```${paramValues}```:表示返回请求参数的一组值.
取用户的请求参数：
```
request.getParameter(String name)   
request.getParameterValues(String name)   
```
使用EL表达式：
```
${param.name}
${paramValues.name}
```   
**其它隐含对象**: cookie、header、headerValues、initParam、pageContext
cookie:
```
${cookie.username}
```   
header：储存用户浏览器和服务端用来沟通的数据.

```
<!--取得用户浏览器版本-->
${header["User-Agent"]}
```   

另外在鲜少机会下，有可能同一标头名称拥有不同的值，此时必须改为使用headValues 来取得这些值.
initParam：取得设定站点的环境参数（Context）
```
<!--一般的方法-->   
String userid = (String)application.getInitParameter("userid");   
<!--使用EL表达式-->
${initParam.userid}
```   
pageContext：取得其他有关用户要求或页面的详细信息。

```
     <!--取得请求的参数字符串-->
     ${pageContext.request.queryString}
     <!--取得请求的URL，但不包括请求之参数字符串-->
     ${pageContext.request.requestURL}         
     <!--服务的web application 的名称-->
     ${pageContext.request.contextPath}          
     <!--取得HTTP 的方法(GET、POST)-->
     ${pageContext.request.method} 
     <!--取得使用的协议(HTTP/1.1、HTTP/1.0)-->
     ${pageContext.request.protocol}        
     <!--取得用户名称-->
     ${pageContext.request.remoteUser}
     <!--取得用户的IP 地址-->
     ${pageContext.request.remoteAddr}
     <!--判断session 是否为新的-->
     ${pageContext.session.new}             
     <!--取得session 的ID-->
     ${pageContext.session.id}
     <!--取得主机端的服务信息-->
     ${pageContext.servletContext.serverInfo}   
```   
**四、EL运算符**

- 算数运算符
+、-、*或$、/或div、%或mod

- 关系运算符
==或eq、!=或ne、<或lt、>或gt、<=或le、>=或ge

- 逻辑运算符
&&或and、||或or、!或not

- 其它运算符
Empty 运算符、条件运算符、()运算符
```
${empty param.name}、${A?B:C}、${A*(B+C)}
```   


**五、EL函数**

EL自定义函数：在EL表达式中调用某个java类的静态方法，这个静态方法需在web应用程序中进行配置才能被EL表达式调用 EL自定义函数可以扩展EL表达式的功能，让EL表达式完成普通java程序代码所能完成的功能。

**EL自定义函数开发步骤**
- 编写EL自定义函数映射的java 类中的静态方法：这个java类必须带有public 修饰符，方法必须是这个类的带有publi 修饰符的静态方法
```
public class MyELFunction {

    public static String concat(String str1,String str2){
        return str1+str2;
    }

}
```
- 编写标签库描述文件（tld文件）,在tld 文件中描述自定义函数  
```
<function>
    <name>concat</name>
    <function-class>com.atguigu.el.MyELFunction</function-class>
    <function-signature>java.lang.String concat(java.lang.String,java.lang.String)</function-signature>
</function>
````
- 在JSP页面中导入和使用EL自定义函数
```
${atguigu:concat(param.name1,param.name2)}  
```