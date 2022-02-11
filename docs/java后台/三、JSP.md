# JSP简介
**一、什么是JSP**     
jsp就是servlet.
第一次请求时，会把jsp 编译成java,再把java编译成.class,然后调用service方法。
第二次请求时，直接调用对应class的service方法.     
Java Server Page：java 服务器端网页，在HTML页面中编写java 代码的页面。   
JSP可以访问在WEB应用程序中除了WEB-INF及其子目录外的其他任何目录   
JSP页面的访问路径与普通HTML的访问路径形式也完全一样  

**二、为什么使用JSP**
- JSP 它本质上是javaServlet,也就是一个普通的java类，拥有各种强大的企业级API.    
- JSP页面可以与处理逻辑的Servlet一起使用，这种模式被javaServlet模板引擎所支持.   
- 与Servlet比，它同样可以修改或者编写HTML页面，而不用去面对大量的println语句.   
- 与javaScript相比，虽然javaScript可以在客户端动态生成HTML，但难以与服务器进行交互，因此不能提供复杂的服务.    

**三、JSP处理流程**   
web 服务器如何使用JSP来创建网页
- 浏览器发送一个浏览器请求给HTTP请求给服务器
- 服务器识别出这是一个对JSP网页的请求，将请求传递给JSP引擎
- JSP引擎从磁盘中载入JSP文件，将其转化为Servlet,这里的转化是将JSP页面中的模板文件用println()输出，JSP页面中嵌入的代码不变
- JSP引擎将Servlet编译成.class文件（可执行类），然后将请求传递给Servlet引擎.
- Web服务器饿的某组件会调用servlet引擎，然后载入并执行Servlet.在执行过程中，Servlet 产生HTML格式的输出并将其嵌入HTTP Response 中上交给服务器.
- Web 服务器以静态HTML网页形式将HTTP Response返回到浏览器中.
- 浏览器处理HTTP Response中动态产生的HTML网页，就好像在处理静态网页一样.   

一般情况下，JSP 引擎会检查 JSP 文件对应的 Servlet 是否已经存在，并且检查 JSP - 文件的修改日期是否早于 Servlet。如果 JSP 文件的修改日期早于对应的 Servlet，那么容器就可以确定 JSP 文件没有被修改过并且 Servlet 有效。这使得整个流程与其他脚本语言（比如 PHP）相比要高效快捷一些。

**JSP 生命周期**

因为JSP本质是一个Servlet,所以JSP的生命周期和Servlet的生命周期是相同的，但JSP的生命周期包括将JSP编译成Servlet的过程.
- 编译
Servlet 容器编译Servlet 源文件，生成Servlet 类（.class文件）,编译分为三步:
    - 解析JSP文件
    - 将JSP转化为Servlet
    - 编译Servlet
- 初始化   
加载与JSP对应的Servlet类，创建其实例，并调用它的初始化方法jspinit()
- 执行   
调用与JSP对应的Servlet实例的服务方法_jspservice()
- 销毁   
调用与JSP对应的Servlet实例的销毁方法jspDestory()
# JSP 语法
1. ```<% 脚本代码 %>``` 或者 ```<jsp:scriptlet>```脚本代码```</jsp:scriptlet>```,这两者是等价的
```
<%`
    System.out.print("hello world");
%>
<jsp:scriptlet>
   System.out.print("hello world");
</jsp:scriptlet>
```
2. ```<%=声明变量或者方法%>```或者```<jsp:declaration>```声明变量或者方法```</jsp:declaration>```
```
<%
    String name = "hello world";
%>
<jsp:declaration>
   String name = "hello world";
</jsp:declaration>
```
3.```<%=表达式%>```或者```<jsp:expression>```表达式```</jsp:expression>```jsp 表达式
表达式包含符合java语言规范的表达式，但不能使用;结束表达式。表达式的值先被转化为String,然后插入到表达式的位置.
```
<%=new Date().toLocaleString()%>

<jsp:expression>
   new Date().toLocaleString()
</jsp:expression>
```  

4.注释   
```<%--注释内容--%>```    
这种注释是jsp 注释，注释内容不会被发送至浏览器，也不会被编译 

```<!--注释内容--!>```   
HTML注释，通过浏览器查看源代码时可以看见注释内容  

# JSP 指令
jsp 提供了三种指令，分别是page、include、taglib      
JSP指令是为JSP引擎而设计的，他们并不直接产生任何可见输出，而只是告诉引擎如何处理JSP页面中的其余部分。      

指定的基本语法格式```<%@ 指令 属性名 =“值”>```

**一、page 指令** 

page 指令用于定义JSP页面的各种属性，无论page指令出现在JSP页面中的什么地方，它作用的都是整个JSP页面，为了保持程序的可读性和遵循良好的编程习惯，page 指令最好是放在整个JSP页面的起始位置      

**page 指令的属性**   
1. language
定义jsp页面所用的脚本语言，默认为java,目前只支持java,所以无意义
```<%@ page language="java" %>```

2. session
指定页面是否使用隐含对象session,默认为true,为false不能使用隐含对象session,但是可以使用通过其它方式获取的session
```<%@ page session="true" %>```

3. import
指定导入的包,可以用","分开,表示导入多个包
```<%@ page import="java.util.Date" %>```

4. contentType
指定当前JSP页面的响应类型。实际调用的是```response.setContentType("text/html;charset=UTF-8")```
```tomcat->config->web.xml```文件里面有，可以去搜一下某种格式的参数。
```<%@ page contentType="text/html;%>```

5. errorPage     
指定当JSP页面发生异常时需要转向的错误处理页面   
```<%@ page  errorPage="error.jsp" %>```   
errorPage 指定若当前页面出现错误的实际响应页面是什么。其中/表示的是当前WEB应用的根目录
在响应error.jsp 时，JSP引擎使用的是请求转发的方式。
还可以在web.xml文件中配置错误页面
```
<error-page>
  <error-code>500</error-code>
  <location>/error.jsp</location>
</error-page>

<error-page>
  <exception-type>java.lang.ArithmeticException</exception-type>
  <location>/error.jsp</location>
</error-page>
```   
6. isErrorPage   
isErrorPage 指定当前页面是否为错误处理页面
```<%@ page  isErrorPage="true" %>```

可以说明当前页面是否可以使用exception隐藏对象，需注意的是：若指定isErrorPage = “true”,并使用exception 的方法了，一般不建议能够直接访问该页面    

那么如何使客户不能访问某个页面呢？      
对于Tomcat 服务器而言，WEB-INF下的文件是不能通过在浏览器中直接输入地址的方式来访问的。    但是可以通过转发的方式来访问      

7. extends   
当前JSP翻译成源文件之后从哪一个类继承，无意义   

8. isELIgnored    
是否执行EL表达式，默认情况下是true,如果在jsp页面中使用EL表达式要设置为false.    
```<%@ page isELIgnored="false"%>```

**二、include 指令**   
include 指令用于通知JSP引擎在翻译当前JSP页面时将其他文件的内容合并进当前JSP页面转换成的Servlet源文件，这种在源文件级别进行引入的方式称之为静态引入，当前JSP页面与静态引入的页面紧密结合为一个Servlet
<@ include file="jsp4.jsp"%>

其中file 属性用于指定被引入文件的相对路径。如果以/开头，表示当前web 应用的根目录。      

被引入的文件必须遵循JSP语法，其中的内容可以包含静态HTML、JSP脚本元素、JSP指令和JSP行为元素等普通JSP页面所具有的一切内容    

被引入的文件可以使用任意的扩展名，即使扩展名时html,JSP引擎也会按照处理JSP页面的方式处理它里面的内容，为了见名知意，JSP规范建议使用.jspf(JSP fragments)作为静态引入文件的扩展名。    

在将JSP文件翻译称Servlet源文件时，JSP引擎将合并被引入的文件与当前JSP页面中的指令元素（设置pageEncoding 属性的page指令除外），所以，除了import 和pageEncoding属性之外，page指令的其他属性不能在这两个页面中有不同的设置值   

**三、taglib 指令**    

JSP API允许用户自定义标签，一个自定义标签库就是自定义标签的集合。
Taglib指令引入一个自定义标签集合的定义，包括库路径、自定义标签。
Taglib指令的语法：
<%@ taglib uri="uri" prefix="prefixOfTag" %>

uri属性确定标签库的位置
prefix属性指定标签库的前缀

# JSP 隐式对象   
jsp 隐式对象是jsp 容器为每个页面提供的java对象，不用声明即可使用,jsp容器为jsp   提供了九个隐式对象：```request,response,session,out,application,config,pageContext,page,Exception```   

**一、request**   
request对象是javax.servlet.http.HttpServletRequest类的实例   
每当客户端请求一个JSP页面时，JSP引擎就会制造一个新的request对象来代表这个请求   
request对象提供了一系列方法来获取HTTP头信息，cookies，HTTP方法等等      

**二、response对象**   
response对象是javax.servlet.http.HttpServletResponse类的实例       
当服务器创建request对象时会同时创建用于响应这个客户端的response对象     
response对象也定义了处理HTTP头模块的接口    
通过这个对象，开发者们可以添加新的cookies，时间戳，HTTP状态码等等   

**三、session**    
session对象是 javax.servlet.http.HttpSession 类的实例。和Java Servlets中的session对象有一样的行为。        
session对象用来跟踪在各个客户端请求间的会话。      

**四、Exception**     
exception 对象包装了从先前页面中抛出的异常信息。它通常被用来产生对出错条件的适当响应。        
这个对象只有在page 指令的isErrorPage属性被设置为true时才可以使用.     

**五、out**
out对象是 javax.servlet.jsp.JspWriter类的实例，用来在response对象中写入内容。     

**六、application**   
application对象直接包装了servlet的ServletContext类的对象，是javax.servlet.ServletContext 类的实例。        
这个对象在JSP页面的整个生命周期中都代表着这个JSP页面。这个对象在JSP页面初始化时被创建，随着jspDestroy()方法的调用而被移除。        
通过向application中添加属性，则所有组成您web应用的JSP文件都能访问到这些属性        

**七、config**   
config对象是 javax.servlet.ServletConfig类的实例，直接包装了servlet的ServletConfig类的对象。    
这个对象允许开发者访问Servlet或者JSP引擎的初始化参数，比如文件路径等   

**八、pageContext**   
pageContext对象是javax.servlet.jsp.PageContext类的实例，用来代表整个JSP页面。    
这个对象主要用来访问页面信息，同时过滤掉大部分实现细节。   
这个对象存储了request对象和response对象的引用。application对象，config对象，session对象，out对象可以通过访问这个对象的属性来导出。   
pageContext对象也包含了传给JSP页面的指令信息，包括缓存信息，ErrorPage URL,页面scope等。   

**九、page**       
这个对象就是页面实例的引用。它可以被看做是整个JSP页面的代表。     
page 对象就是this对象的同义词。

# JSP动态标签
JSP动态标签也成为JSP动作元素，它是在请求处理阶段起作用，它使用XML语法写成的。   
jsp 标签基本都是预定义的函数，jsp 规范定义了一系列标准动作，它用jsp做前缀，   

**1.jsp:include**    
在页面被请求的时候引入一个文件    
```include``` 指令是在编译的时候就将被引入的文件编译进当前文件，最后生成一个Servlet.只能引入遵循jsp文件.   
```jsp```：include标签是在请求当前页面时插入被访问页面的输出内容，被动态引入的资源必须是一个能独立被WEB容器调用和执行的资源.最后生成的是两个独立的Servlet.     

属性作用page被引入的文件的相对URL地址flush布尔属性，定义在包含资源前是否刷新缓存区
```<jsp:include page = "b.jsp" flush="true"/>```  

**2. javaBean相关的动态标签 **   
JSP规范中专门定义了三个JSP标签:```<jsp:useBean>,<jsp:getProperty>,<jsp:setProperty>.```   

分别用于创建和查找javaBean 的实例对象、设置javaBean对象的属性、读取javaBean对象的属性.   

- 对于JSP页面来说，只要一个类具有一个公共的、无参数的构造方法，就可以把这个类当作javaBean 来使用,如果类中有不接受任何参数的getter方法或只接受一个参数的setter 方法，就可以把前缀get或set后面的部分当着一个属性名来引用。    

- JSP页面可以像调用一个普通java 类的方式去调用javaBean,即先使用java代码创建javaBean的实例对象，然后直接调用javaBean对象的getter方法和setter方法.

**<jsp:useBean />标签**      

用于在某个指定的域范围（application,session,request,pageContext）中查找一个指定名称的javaBean对象,如果存在则直接返回该javaBean对象的引用,如果不存在则实例化一个新的javaBean对象并将它按指定的名称存储在指定的域范围中.    
它有几个属性：
|属性|作用|
|-|-|   
|id|用于指定javaBean的实例对象的引用名称和其存储在域范围中的名称|
|.class|用于指定javaBean的完整类名（即必须带有包名）|   
|scope|用于指定javaBean 实例对象所存储的域范围，其取值只能是page,request,session,application，四个值中的一个,默认值是page|   
|beanName|当没有这个Bean 的时候通过反射创建一个类型为type 的Bean|
|type|指定引用该对象的变量的类型，它必须是Bean类的名字、超类名字、该类所实现的接口名字之一。请记住变量的名字是由id属性指定的。|
 
```
<jsp:useBean id="myName" class="" scope=""/>
<jsp:useBean id="myName" beanName="全类名" type="" scope=""/>
```
**<jsp:setProperty />**

用来设置已经实例化的Bean对象的属性,有两种使用方式：
1. 写在jsp:useBean外面
```
<jsp:useBean id="myName" ... />
<jsp:setProperty name="myName" property="someProperty" .../>
```
这种情况下，不管jsp:useBean是新建了一个bean还是找到了一个现有的bean,jsp:setProperty都会执行
2. 写在在jsp:useBean里面
```
<jsp:useBean id="myName" ... >
    <jsp:setProperty name="myName" property="someProperty" .../>
</jsp:useBean>
```
这种情况下，只有jsp:useBean新建了一个bean,jsp:setProperty才会执行。如果是找到一个现有的bean，则jsp:setProperty不执行.
jsp:setProperty 有四个属性：
|属性|作用|
|-|-|   
|name|name属性是必需的。它表示要设置属性的是哪个Bean.|
|property|）|property属性是必需的。它表示要设置哪个属性。有一个特殊用法：如果property的值是"*"，表示所有名字和Bean属性名字匹配的请求参数都将被传递给相应的属性set方法。|      
|value|value 属性是可选的。该属性用来指定Bean属性的值。字符串数据会在目标类中通过标准的valueOf方法自动转换成数字、boolean、Boolean、 byte、Byte、char、Character。例如，boolean和Boolean类型的属性值（比如"true"）通过 Boolean.valueOf转换，int和Integer类型的属性值（比如"42"）通过Integer.valueOf转换。 　　value和param不能同时使用，但可以使用其中任意一个。|  
|param|param 是可选的。它指定用哪个请求参数作为Bean属性的值。如果当前请求没有参数，则什么事情也不做，系统不会把null传递给Bean属性的set方法。因此，你可以让Bean自己提供默认属性值，只有当请求参数明确指定了新值时才修改默认属性值。|  

**<jsp:getProperty />**    
|属性|作用|
|-|-|
|name|bean的id|
|property|bean 的属性名|    

```<jsp:getProperty name="bean 的 id" property="属性名"/>```

**3.jsp:forward**     
jsp:forward动作把请求转到另外的页面。jsp:forward标记只有一个属性page，代表要转发的页面的相对路径.page的值既可以直接给出，也可以在请求的时候动态计算，可以是一个JSP页面或者一个 Java Servlet.    
```
<jsp:forward page="相对 URL 地址" />
```   

# JSP自定义标签
- 自定义标签可以降低JSP开发的复杂度和维护量，从HTML角度来说，可以使HTML不用去过多的关注那些比较复杂的商业逻辑（业务逻辑）.

- 利用自定义标签，可以软件开发人员和页面设计人员合理分工：页面设计人员可以把精力集中在使用标签（HTML,XML,JSP）创建的网站上，而软件开发人员则可以将精力集中在实现底层功能上面，如国际化等，从而提高了工程生产力.

- 将具有共同特性的tag库应用于不同的项目中，体现了软件复用的思想.

**什么是自定义标签**    
用户定义的一种自定义的jsp 标记，当一个含有自定义标签的jsp页面被jsp引擎编译成servlet时，tag标签被转化成了对一个成为标签处理类(TagHandler)对象的操作。于是,当jsp页面被jsp引擎转化为servlet后，实际上tag标签被转化为了对tag处理类的操作.
这是JSTL中定义的标签forEach
```
    <name>forEach</name>
    <tag-class>org.apache.taglibs.standard.tag.rt.core.ForEachTag</tag-class>
    <tei-class>org.apache.taglibs.standard.tei.ForEachTEI</tei-class>
    <body-content>JSP</body-content>
```   

**标签库API**   
**简单标签和传统标签**    
- 开发自定义标签，其核心就是要编写处理器类，一个标签对应一个标签处理器类，而一个标签库则是很多标签处理器的集合。所有的标签处理器类都要实现JspTag接口，该接口中没有定义任何方法，主要作为Tag和SimpleTag 接口的父接口。
- 在jsp 2.0 以前，所有标签处理器类都必须实现Tag 接口，这样的标签称为传统标签.
- jsp 2.0 规范又定义了一种新的类型的标签，称为简单标签，其对应的处理器类要实现SimpleTag 接口.

2. 标签的形式
- 空标签
```
<hello/> 
```
- 带有属性的空标签   

```
<max num1="3" num2="5">
```
- 带有内容的标签

```
<greeting>
    hello
</greeting>
```
- 带有内容和属性的标签
```
<greeting name="Tom">
    hello
</greeting>
```   

**一、自定义标签的开发与应用步骤**     
编写完成标签功能的 java 类（标签处理器）:实现SimpleTag 接口
```
public class HelloSimpleTag implements SimpleTag {
    /**
     * 该方法用于完成所有的标签逻辑。该方法可以抛出javax.servlet.jsp.SkipPageException 异常，
     * 用于通知web容器不再执行JSP页面中位于结束标记后面的内容 
     * 容器调用标签处理器对象的doTag 方法执行标签逻辑
     */
    @Override
    public void doTag() throws JspException, IOException {
        System.out.println("doTag");
    }
    /**
     * 该方法把父标签处理器对象传递给当前标签处理器对象
     * JSP引擎对父标签处理器对象传递给当前标签处理器对象，只有存在父
     * 签时，JSP引擎才会调用该方法.
     */
    @Override
    public void setParent(JspTag jspTag) {
        System.out.println("setParent");
    }
    /**
     * 该方法用于活的标签的父标签处理器对象
     */
    @Override
    public JspTag getParent() {
        System.out.println("getParent");
        return null;
    }
    private PageContext pageContext;
    /**
     * 该方法把代表JSP页面的pageContext 对象传递给处理器对象
     * JSP引擎代表JSP页面的pageContext 对象传递给标签处理器对象,
     * pageContext 是jspContext 的子类
     */
    @Override
    public void setJspContext(JspContext jspContext) {
        this.pageContext = (PageContext) jspContext;
        System.out.println("setJspContext");
    }
    /**
     * 该方法把用于把代表标签体的JSPGragment对象传递给标签处理器对象
     * 若存在标签体，JSP引擎将把标签体封装成一个JspFragment对象
     * 调用setJspBody方法将JspFragment对象传递给标签处理器对象
     * 若标签体为空，这setJspBody 将不会被JSP引擎调用.
     */
    @Override
    public void setJspBody(JspFragment jspFragment) {
    }
}
```  

SimpleTag 的生命周期
- 每次遇到标签时，容器构造一个SimpleTage的实例，并且这个构造方法没有参数。和经典的标签是一样的，SimpleTag不能进行缓冲，故不能重用，每次都需要构造新的实例
- 调用构造方法后，就用setJspContext()和setParent()方法，只有这个标签有父标签时，才调用setParent()方法。
- 容器调用每个属性的setter方法以设置这些属性的值。
- 如果存在Body，那么就使用setJspBody方法设置这个标签的标签体。
- 容器调用doTag方法，所有的标签的逻辑，迭代和Body计算都在这个方法中。
- 当doTag方法返回时，所有参数都被锁定。

**SimpleTagSupport 类**       
通常情况下开发简单标签直接继承SimpleTagSupport就可以了，可以直接调用其对应的getter方法得到对应api.
编写标签库描述（tld）文件，在tld 文件中对自定义中进行描述
- 在WEB-INF文件夹下新建一个.tld 的XML文件，导入固定的部分

```
<?xml version="1.0" encoding="ISO-8859-1"?>
<taglib xmlns="http://java.sun.com/xml/ns/j2ee"
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:schemaLocation="http://java.sun.com/xml/ns/j2ee http://java.sun.com/xml/ns/j2ee/web-jsptaglibrary_2_0.xsd"
        version="2.0">

    <!--描述TLD文件-->
    <description>MyTag 1.0</description>
    <display-name>MyTag core</display-name>
    <!--自定义标签库的版本号-->
    <tlib-version>1.0</tlib-version>

    <!--建议在JSP页面上使用的标签的前缀-->
    <short-name>atguigu</short-name>
    <!--作为 tld 文件的 id,用来唯一表示当前的 tld 文件，多个 tld 文件的 uri 不能重复，通过 JSP 页面的 taglib 标签的 uri 属性来引用-->
    <uri>http://www.atguigu.com/mytag/core</uri>
</taglib>
```  

- 在tld 文件中描述自定义的标签
```
  <!--描述自定义的HelloSimpleTag标签-->
    <tag>
        <!-- 标签名：在jsp页面上使用标签时的名字 -->
        <name>hello</name>
        <!-- 标签所在的全类名 -->
        <tag-class>com.atguigu.tag.HelloSimpleTag</tag-class>
        <!-- 标签体类型-->
        <body-content>empty</body-content>
    </tag>
```
- 在JSP页面导入和使用自定义标签
    - 使用 taglib 指定导入标签库描述文件

```
    <%@taglib prefix="atguigu" uri="http://www.atguigu.com/mytag/core" %>
```

- 使用自定义的标签

```
    <atguigu:hello/>
```
setJspContext:一定会被JSP引擎所调用，先于doTag,把代表JSP引擎的pageContext传给标签处理器类.
```
    private PageContext pageContext;   
    @Override
    public void setJspContext(JspContext jspContext) {
        this.pageContext = (PageContext) jspContext;
    }
```   
二、带属性的自定义标签
先在标签处理器类中定义 setter方法 方法.建议把所有的属性类型都设置为String 类型，
```
    private String value;
    private String count;

    public void setValue(String value) {
        this.value = value;
    }

    public void setCount(String count) {
        this.count = count;
    }
```

2.在tld 描述文件中来描述属性
```
        <!--描述当前标签的属性-->
        <attribute>
            <!--属性名-->
            <name>value</name>
            <!--该属性是否必须-->
            <required>true</required>
            <!--
            rtexprvalue runtime expression value
            当前属性是否可以接收运行时表达式的动态值
            -->
            <rtexprvalue>true</rtexprvalue>
        </attribute>
        <attribute>
            <name>count</name>
            <required>false</required>
            <rtexprvalue>false</rtexprvalue>
        </attribute>
```
3.使用
属性名同tld 文件中定义的名字
```
    <atguigu:hello value="atguigu" count="10"/>
```
**练习**    
定制一个带有两个属性的标签,用于计算并输出两个数的最大值.
写标签处理器类
```
public class MaxTag extends SimpleTagSupport {
    private String num1;
    private String num2;

    public void setNum1(String num1) {
        this.num1 = num1;
    }
    public void setNum2(String num2) {
        this.num2 = num2;
    }
    @Override
    public void doTag() throws JspException, IOException {
        int n1 = Integer.parseInt(num1);
        int n2 = Integer.parseInt(num2);
        PageContext pageContext = (PageContext) getJspContext();
        JspWriter out = pageContext.getOut();
        try {
           out.print(n1 > n2 ? n1 : n2);

        } catch (IOException e) {
            out.print("输入的格式不正确");
        }
        out.print("<br><br>");
    }
}
```
在tld 描述文件中描述属性
```
<tag>
        <!-- 标签名：在jsp页面上使用标签时的名字 -->
        <name>maxtag</name>
        <!-- 标签所在的全类名 -->
        <tag-class>com.atguigu.tag.MaxTag</tag-class>
        <!-- 标签体类型 -->
        <body-content>empty</body-content>

        <!--描述当前标签的属性-->
        <attribute>
            <name>num1</name>
            <required>true</required>
            <rtexprvalue>true</rtexprvalue>
        </attribute>
        <attribute>
            <name>num2</name>
            <required>true</required>
            <rtexprvalue>true</rtexprvalue>
        </attribute>
    </tag>
```
在jsp页面调用
```
  <atguigu:maxtag num1="${param.a}" num2="${param.b}"/>
``` 
定制一个带有一个属性的标签<atguigu:readFile src="">,用于输出指定文件的内容.
定义标签处理器类
```
public class ReadFileTag extends SimpleTagSupport {

    private String src;

    public void setSrc(String src) {
        this.src = src;
    }

    @Override
    public void doTag() throws JspException, IOException {
        PageContext pageContext = (PageContext) getJspContext();
        InputStream in = pageContext.getServletContext().getResourceAsStream(src);
        BufferedReader reader = new BufferedReader(new InputStreamReader(in));
        String str = null;
        while((str = reader.readLine()) != null){
            pageContext.getOut().print(str);
            pageContext.getOut().print("<br>");
        }
    }
}
``` 
在tld 文件中添加描述
```
    <tag>
        <name>readfile</name>
        <tag-class>com.atguigu.tag.ReadFileTag</tag-class>
        <body-content>empty</body-content>
        
        <attribute>
            <name>src</name>
            <required>true</required>
            <rtexprvalue>true</rtexprvalue>
        </attribute>
    </tag>
```
在页面导入并使用
```
<%@ taglib prefix="atguigu" uri="http://www.atguigu.com/mytag/core"%>
<atguigu:readfile src="/WEB-INF/note.txt"/>
```
**三、带标签体的自定义标签**     

- 在自定义标签的标签处理器中使用JspFragment 对象封装标签体信息
- 若配置了标签含有标签体，则Jsp引擎会调用setJspBody()方法把JspFragment传递给标签处理器类，在SimpleTagSupport 中还定义了一个getJspBody()方法
，用于返回JspFragment 对象
- JspFragment 的invoke(Writer)方法：把标签体内容从Writer 中输出，若为null,则等同于invoke(gerJspContext().getOut())，即直接把标签体内容输出到页面上
- 在tld文件中，使用body-content 节点来描述标签体的类型：
指定标签体的类型，大部分情况下，取值为scriptless，可能取值有3种
|属性|说明| 
|-|-|   
|empty|没有标签体|
|scriptless|标签体可以包含el表达式和JSP动作元素，但不能包含JSP的脚本元素|   
|tagdependent|在标签体重所有的代码都会原封不动的交给标签处理器，而不是将执行结果传递给标签处理器|   


**练习**
定义一个自定义标签，<atguigu:print time="10">abcde</atguigu:print>,把标签体内容转换为大写并输出 time 次到浏览器上
1. 定义标签体
```
public class PrintUpperTag extends SimpleTagSupport {
    private String time;
    public void setTime(String time) {
        this.time = time;
    }

    @Override
    public void doTag() throws JspException, IOException {
        //1.得到标签体的内容
        JspFragment body = getJspBody();
        StringWriter writer = new StringWriter();
        body.invoke(writer);
        String content = writer.toString();

        //2.变为大写
        content = content.toUpperCase();
        //3.得到out隐含变量
        //4.循环输出
        int count = 1;
        try {
            count = Integer.parseInt(time);
        }catch (Exception e){

        }
        for(int i = 0;i < count;i++){
            getJspContext().getOut().print((i+1)+"."+content+"<br>");
        }
    }
}
``` 
2. 添加描述文件
```
    <tag>
        <name>printUpper</name>
        <tag-class>com.atguigu.tag.PrintUpperTag</tag-class>
        <body-content>scriptless</body-content>
        <attribute>
            <name>time</name>
            <required>true</required>
            <rtexprvalue>true</rtexprvalue>
        </attribute>
    </tag>
```
3. 使用
```
<atguigu:printUpper time="10">hello world</atguigu:printUpper>
```
**四、带父标签的自定义标签**
- 父标签无法获取子标签的引用，父标签仅把子标签作为标签体来使用.
- 子标签可以通过getParent（）获取父标签的引用，(需继承SimpleTagSupport或自己实现SimpleTag接口该方法)若子标签确有父标签，JSP引擎会把代表父标签的引用通过setParent（JspTag）赋给标签处理器
- 注意：父标签的类型是jspTag 类型.该接口是一个空接口，是用来统一SimpleTag 和Tag 的实际使用需要进行类型的强制转换。
- 在tld 配置文件中，无需为父标签有额外的配置，但子标签是以标签体的形式存在的，所以父标签的body-content需要设置为scriptless
1. 父标签
```
public class ParentTag extends SimpleTagSupport {
    private String name = "ATGUIGU";

    public void setName(String name) {
        this.name = name;
    }

    public String getName() {
        return name;
    }
    @Override
    public void doTag() throws JspException, IOException {
        System.out.println("父标签处理器类name:"+name);
        getJspBody().invoke(null);
    }
}
```
2. 子标签
```
public class SonTag extends SimpleTagSupport {
    @Override
    public void doTag() throws JspException, IOException {
        //1.得到父标签的引用
        JspTag parent = getParent();
        //2.获取父标签的属性
        ParentTag parentTag = (ParentTag) parent;
        String name = parentTag.getName();
        //3.把name值打印到JSP页面上
        getJspContext( ).getOut().print("子标签输出name:"+name);
    }
}
```
3.添加描述文件
```
<tag>
    <name>parentTag</name>
    <tag-class>com.atguigu.tag.ParentTag</tag-class>
    <body-content>scriptless</body-content>
</tag>
<tag>
    <name>sonTag</name>
    <tag-class>com.atguigu.tag.SonTag</tag-class>
    <body-content>empty</body-content>
</tag>
```
4. 使用
```
<!--父标签打印name到控制台-->
<atguigu:parentTag>
    <%--子标签以父标签的标签体存在，子标签把父标签的name打印到JSP页面上--%>
    <atguigu:sonTag/>
</atguigu:parentTag>
```
**五、EL自定义函数**   
EL自定义函数：在EL表达式中调用某个java类的静态方法，这个静态方法需在web应用程序中进行配置才能被EL表达式调用   
EL自定义函数可以扩展EL表达式的功能，让EL表达式完成普通java程序代码所能完成的功能。
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
```  

- 在JSP页面中导入和使用EL自定义函数
```
  ${atguigu:concat(param.name1,param.name2)}
```
# JSTL  
JSTL全名为JavaServer Pages Standard Tag Library,它是由JCP（Java Community Process）所指定的标准规格，主要是提供给Web 开发人员一个标准通用的标签函数库。
主要分为五大类：
1. 核心标签库：c(前缀)```http://java.sun.com/jsp/jstl/core(url)```
2. 118N标签库：fmt```http://java.sun.com/jsp/jstl/fmt```
3. sql标签库： sql ```http://java.sun.com/jsp/jstl/sql```
4. xml标签库： x ```http://java.sun.com/jsp/jstl/xml```
5. 函数标签库：fn ```http://java.sun.com/jsp/jstl/functions```   

在这里我们主要使用的是核心标签库.   
功能分类：   
1. 表达式操作
2. 流程操作
3. 迭代操作
4. URL操作    

**一、表达式操作**  
- <c:out>
主要用来显示数据的内容，相当于```<%= %>```    

**属性**   
|属性|说明|EL表达式|类型|必须|默认值|   
|-|-|-|-|-|-|   
|value|需要显示出来的值	|Y|Object|是|无|
|default|如果value为null，则显示这个值|Y|Object|否|无|   
|escapeXml|是否转换特殊字符	|Y|boolean|否|true|      



```
 <%
        request.setAttribute("book","<<java>>");
        %>
        <!--敏感字符不转义,翻译到网页为book:<<java>>-->
        book:${requestScope.book}<br>
        <!--敏感字符转义，翻译到网页为book:&lt;&lt;java&gt;&gt;-->
        book:<c:out value="${requestScope.book}"/>
```
- <c:set>    

主要是用来将变量存储至JSP范围中或是JavaBean 的属性中.

**属性**
|名称	|说明	|EL表达式	|类型	|必须|	默认值|
|-|-|-|-|-|-|    
|value|	要被存储的值|	Y|	Object|	否	|无|
|var|	欲存入变量的名称|	N	|String|	否	|无|   
|scope	|var 变量的JSP范围	|N|	String|	否	|page|
|target	|为一JavaBean或java.util.Map对象	|Y|	Object|	否	|无|
|property|	指定target对象的属性	|Y|	String|	否	|无|


1. 将value 值存储至范围为scope的varName 变量之中,value 可以使用EL表达式
 
   <c:set var="a" value="123" scope="page"></c:set>
        a:${pageScope.a}
        <%--
            //相当于
            pageContext.setAttribute("a","134");
        --%>
        <%--使用EL表达式--%>
        <c:set var="name" value="${param.name}" scope="request"></c:set>
        name:${requestScope.name}<br><br>

2. 将本体内容的数据存储至范围为scope 的varName 变量之中

```
<c:set var="b" scope="request">
            这里是b
        </c:set>
        b:${requestScope.b}
```
3. 将value 的值储存至target 对象的属性中

```
<%
    Customer customer = new Customer();
    customer.setId(34);
    request.setAttribute("cust",customer);
%>
        <c:set target="${requestScope.cust}" property="id" value="${param.id}"></c:set>
        ID:${requestScope.cust.id}
```
4. 将本体的内容存储至target对象的属性中
```
    <s:set target="${requestScope.cust}" property="phone">
                12345
            </s:set>
            Phone:${requestScope.cust.phone}<br><br>
    <c:remove>
```
5. 移除指定的变量
```
        <c:set var="data" value="2020-4-3" scope="session"/>
        data:${sessionScope.data}<br><br>
        <c:remove var="data" scope="session"/>
        移除后data:${sessionScope.data}
```

**二、流程控制** 
- c:if   

|名称	|说明	|EL表达式	|类型	|必须|	默认值|
|-|-|-|-|-|-|    
|test	|如果表达式的结果为true，则执行本体内容，false则相反	|Y|	boolean|	是|	
|var|	用来存储test 运算后的结果，即true或false	|N|	String	|否|	
|scope|	var 变量的JSP范围	|N|	String	|否| 	

```
    <c:set var="age" value="20" scope="request"/>
    <c:if test="${requestScope.age > 18}">成年了！</c:if><br>
    <c:if test="${param.age > 18}" var="isAdult" scope="request"></c:if>
    isAdult:<c:out value="${requestScope.isAdult}"/>
```
<c:if> 标签必须要有test 属性，当test中的表达式结果为true时，则会执行本体内容：如果为false,则不会执行.  

除了test属性之外，<c:if> 还有另外两个属性var和scope,当我们执行<c:if> 的时候，可以将这次判断后的结果存放到属性var里，scope 则是设定var. 的属性范围。例如：当表达式过长时，我们会希望拆开处理，或是之后还须使用此结果时，也可以用它先将结果暂时保留，以便日后使用.
 
- c:c:choose、c:when、c:otherwise       
1. c:choose、c:when、c:otherwise可以实现if……else if……else的效果，但较为麻烦.
2. c:choose 以c:when和c:otherwise的父标签出现。c:when和c:otherwise不能脱离c:choose单独使用.
3. c:otherwise必须在c:when之后使用.
4. 当所有的c:when的test 为false时，则执行c:otherwise本体内容
```  
        <c:choose>
            <c:when test="${param.age > 60}">
                老年
            </c:when>
            <c:when test="${param.age > 35}">
                中年
            </c:when>
            <c:when test="${param.age > 18}">
                青年
            </c:when>
            <c:otherwise>
                未成年
            </c:otherwise>
        </c:choose>
```

**三、迭代操作**
- c:forEach
```c:forEach``` 为循环控制，它可以将集合（Collection）中的成员循序浏览一遍，运作方式为当条件符合时，就会持续重复执行```<c:forEach>```的本体内容.

|名称|	说明|	EL表达式	|类型	|必须	|默认值|
|-|-|-|-|-|-|      
|var	|用来存放现在指到的成员	|N|	String|	否	|无|
|items	|被迭代的集合对象	|Y|	Arrays、Collection、Iterator、Enumeration、Map、String	|否	|无|
|varStatus	|用来存放现在指到的相关成员信息	|N|	String|	否	|无|
|begin	|开始的位置|	Y|	int|	否|	0|
|end	|结束的位置|	Y|	int|	否|	最后一个成员|
|step|	每次迭代的间隔数	|Y|	int|	否	|1|     

varStatus 是一个内部类，它的属性如下：   
|属性	|类型	|意义|
|-|-|-|
|index|    	number|	现在指到成员的索引
|count|	number|	总共指到成员的总数|
|first|	boolean|	|现在指到的成员是否为第一个成员|
|last	|boolean	|现在指到的成员是否为最后一个成员
|current|	number	|当前迭代的项
|begin	|number	|begin 的值
|end	|number	|end 的值
|step	|number|	step 的值

**遍历数字**
```
<c:forEach begin="1" end="10" step="3" var="i">
    ${i}--
 </c:forEach>
```   

**遍历Collection**
```
<%
    List<Customer> list = new ArrayList<>();
    list.add(new Customer(1,"AAA"));//index:0
    list.add(new Customer(2,"BBB"));
    list.add(new Customer(3,"CCC"));
    list.add(new Customer(4,"DDD"));
    list.add(new Customer(5,"EEE"));
    list.add(new Customer(6,"FFF"));
    request.setAttribute("custs",list);
%>

<c:forEach items="${requestScope.custs}" var="cust" begin="1" step="2" end="5">
    ${cust.id}:${cust.name}<br>
</c:forEach><br>
<%--使用varStatus--%>
<c:forEach items="${requestScope.custs}" varStatus="status">
    ${status.index}.${status.count}${status.first}.${status.last}${cust.id}:${cust.name}<br>
</c:forEach>
```    

**遍历Map**
```
<%
    Map<String,Customer> map = new HashMap();
    map.put("a",new Customer(1,"AAA"));//index:0
    map.put("b",new Customer(2,"BBB"));
    map.put("c",new Customer(3,"CCC"));
    map.put("d",new Customer(4,"DDD"));
    map.put("e",new Customer(5,"EEE"));
    map.put("f",new Customer(6,"FFF"));
    request.setAttribute("map",map);
%>
<c:forEach items="${requestScope.map}" var="map">
    ${map.key}-${map.value.id}-${map.value.name}
</c:forEach>
```
**遍历数组**
```
<%
    String[] array = {"A", "B", "C"};
    request.setAttribute("array",array);
%>
<c:forEach var="name" items="${requestScope.array}">
    ${name}-
</c:forEach>
```

**遍历Eumberations**
```
<c:forEach var="attrName" items="${pageContext.session.attributeNames}">
    ${attrName}-
</c:forEach>
```

- c:forTokens   
用来浏览一字符串中所有的成员，其成员是由定义符号所分割的.  

|名称	|说明	|EL表达式	|类型	|必须	|默认值|
|-|-|-|-|-|-|
|items	|被迭代的字符串|	N	|String	|否	|无|
|var	|用来存放现在指到的成员|	N	|String|	否	|无|
|delims|	分隔符		|String	|是	|无|

```
<c:set value="a,b,c,d,e,f" var="test" scope="request"></c:set>
<c:forTokens items="${requestScope.test}" delims="." var="s">
    ${s}-
</c:forTokens>
```   

**四、URL操作**

JSTL包含三个与URL操作相关的标签，分别为:```<c:import>、<c:redirect>``````<c:yrl>```.它们的主要功能是：用来将其他文件的内容包含起来、网页的导向，还有URL的产生.
- <c:import>
可以把其它静态或动态文件包含至本身JSP网页，它和JSP动作标签的jsp:include 最大的差别在于：jsp:include 只能包含和自己同一个web application下的文件：而<c:import>除了能包含和自己用一个web application的文件外，亦可以包含不同web application或者是其他网站的文件.       

|名称	|说明	|EL表达式	|类型	|必须	|默认值|
|-|-|-|-|-|-|
|url	|文件被包含的地址	|Y	|String|	是	|无|
|context	|相同Container下,其他web战天必须以"/" 开头	|Y|	String|	否|	无|
|var	|储存好被包含的文件的内容（以String类型存入）|	N|	String|	否|	无|
|scope	|var 变量的JSP范围|	N|	String|	否	|page|
|charEncoding|	被包含文件之内容的编码格式|	Y	|String|	否	|无|
|varReader|	储存被包含的文件的内容（以Reader类型存入）	|N|	String|	否	|无|   

```
    <c:import url="http://www.baidu.com"></c:import>
```
- 如果url 为null或为空时，会产生 JspException   
如果以“/”开头，那么表示跳到web站台的根目录下
- c:url   
c:url 产生一个url地址，可以检查Cookie 是否可用来智能进行 url 重写，对get 请求的参数进行编码

|名称	|说明|	EL表达式|	类型|	必须	|默认值|
|-|-|-|-|-|-|
|value	|执行的URL|	Y|	String|	是	|无
|context|	相同的Container 下，其他web 站台必须以“/”开头	|Y|	String|	否	|无
|var|	储存被包含文件的内容（以String 类型存入）	|N	|String|	否|	无
|scope|	var 变量的JSP范围|	N	|String|	否	|page

```
<c:url value="/test.jsp" var="testurl">
    <c:param name="name" value="尚硅谷"></c:param>
</c:url>
```
- c:redirect    
将客户端的请求从一个JSP网页导向其他文件.
```
<c:redirect url="test2.jsp"></c:redirect>
```

> 
/ 代表的是Web 应用的根目录,response.sendRedirect("/"),这个里面的/代表web 站点的根目录.如果/交由jsp引擎或者tomcat服务器去解析，那么/代表web应用根目录,否则代表web站点根目录