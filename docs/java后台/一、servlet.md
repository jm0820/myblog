# Servlet 简介
**什么是Servlet**      

Java Servlet 是运行在 Web 服务器或应用服务器上的程序，也就是一个java 类，它是作为来自 Web 浏览器或其他 HTTP 客户端的请求和 HTTP 服务器上的数据库或应用程序之间的中间层。

使用 Servlet，您可以收集来自网页表单的用户输入，呈现来自数据库或者其他源的记录，还可以动态创建网页

**Servlet 架构**  

- 第一个到达服务器的 HTTP 请求被委派到 Servlet 容器。
- Servlet 容器在调用 service() 方法之前加载 Servlet。
然后 Servlet 容器处理由多个线程产生的多个请求，每个线程执行一个单一的 Servlet 实例的 service() 方法。

 ![servlet 架构](https://img-blog.csdnimg.cn/20200319220625302.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0FseXNvbl9qbQ==,size_16,color_FFFFFF,t_70)

**Servlet 任务**
- 读取客户端（浏览器）发送的显式的数据。这包括网页上的 HTML 表单，或者也可以是来自 applet 或自定义的 HTTP 客户s端程序的表单。
- 读取客户端（浏览器）发送的隐式的 HTTP 请求数据。这包括 - cookies、媒体类型和浏览器能理解的压缩格式等等。
- 处理数据并生成结果。这个过程可能需要访问数据库，执行 RMI 或 CORBA 调用，调用 Web 服务，或者直接计算得出对应的响应。
- 发送显式的数据（即文档）到客户端（浏览器）。该文档的格式可以是多种多样的，包括文本文件（HTML 或 XML）、二进制文件（GIF 图像）、Excel 等。
- 发送隐式的 HTTP 响应到客户端（浏览器）。这包括告诉浏览器或其他客户端被返回的文档类型（例如 HTML），设置 cookies 和缓存参数，以及其他类似的任务。

**生命周期**   
Servlet 的生命周期是由Web 容器来管理的。

- Servlet 通过调用 init () 方法进行初始化。
```
public void init(ServletConfig servletConfig)
```   

只被调用一次，在创建好实例后立即被调用，用于初始化当前Servlet

- Servlet 调用 service() 方法来处理客户端的请求。
```
public void service(ServletRequest servletRequest, ServletResponse servletResponse)
```    
一般来说service 方法处理具体业务的，每次访问都会被调用，是多线程的。   
- Servlet 通过调用 destroy() 方法终止（结束）。   
```
public void destroy()
```   
destory 是销毁方法，每次当Servlet 服务器关闭时调用。

- 最后，Servlet 是由 JVM 的垃圾回收器进行垃圾回收的。   

**Servlet 容器响应客户请求的过程**   
1. Servlet 引擎检查是否已经装载并创建了该Servlet 的实例对象，如果是，则执行第④步，否则执行第②步。
2. 装载并创建该Servlet 的一个实例对象：调用该Servlet 的构造器。
3. 创建一个用于封装请求的ServletRequest  对象和一个代表响应消息的ServletReqsponse对象，然后调用Servlet的service()方法并将请求和响应对象作为参数传递进去。
4. WEB应用程序被停止或重新启动之前，Servlet引擎将卸载Servlet,并在卸载之前调用Servlet 的destory()方法。
   


# Servlet 使用
准备条件：
引入servlet 的jar 包
```
<dependency>
      <groupId>javax.servlet</groupId>
      <artifactId>javax.servlet-api</artifactId>
      <version>3.1.0</version>
      <scope>provided</scope>
</dependency>
```   
新建一个Servlet
有三种新建Servlet 的方法

- 实现 Servlet 接口
- 继承 GenericServlet抽象类
- 继承 HttpSerevlet 类（开发中常用的方法）
这里实现 Servlet 接口来新建一个Servlet.
```
public class HelloServlet implements Servlet {

    @Override
    public void init(ServletConfig servletConfig) throws ServletException {
        System.out.println("init");
    }

    /**
     * ServletConfig :封装了Servlet 的配置信息，并且可以获取ServleteContext 对象
     *
     */
    @Override
    public ServletConfig getServletConfig() {
        System.out.println("getServletConfig");
        return null;
    }

    @Override
    public void service(ServletRequest servletRequest, ServletResponse servletResponse) throws ServletException, IOException {
        System.out.println("service");
    }

    @Override
    public String getServletInfo() {
        System.out.println("getServletInfo");
        return null;
    }

    @Override
    public void destroy() {
        System.out.println("destroy");
    }

    public HelloServlet() {
        System.out.println("HelloServlet constuct");
    }
}
```

**注册Servlet**   

Servlet 程序必须在WEB应用程序的web.xml文件中进行注册和映射其访问路径，才可以被Servlet 引擎加载和被外界访问   
```
<!--配置和映射Servlet-->
  <servlet>
    <!--Servlet 注册的名字-->
    <servlet-name>Hello</servlet-name>
    <!--Servlet 的全类名-->
    <servlet-class>com.atguigu.HelloServlet</servlet-class>
    <!--可以指定servlet被创建的时机-->
    <!--<load-on-startup>1</load-on-startup>-->
  </servlet>

  <servlet-mapping>
    <!--需要和某个Servlet 节点的Servlet-name子节点一致-->
    <servlet-name>Hello</servlet-name>
    <!--映射具体的访问路径，/代表当前web应用的根目录-->
    <url-pattern>/hello</url-pattern>
  </servlet-mapping>
```   

**Servlet 映射细节**   
一个Servlet 可以被映射到多个url,即多个元素的子元素的设置值可以是同一个Servlet 的注册名
```
<servlet-mapping>
    <servlet-name>Hello</servlet-name>
    <url-pattern>/hello</url-pattern>
  </servlet-mapping>
  <servlet-mapping>
    <servlet-name>Hello</servlet-name>
    <!--映射具体的访问路径
    <url-pattern>/hello2</url-pattern>
  </servlet-mapping>
```

在这个例子中访问hello 和hello2 都调用的是 hello 这个servlet   

- 在Servlet 映射到的URL中也可以使用通配符，但是只能有两种固定的格式：一种格式是“.扩展名”，另一种格式是以政正斜杠/开头并以“/*”结尾

```
*.html   *.do   *.jsp
/action/*   以/action/ 开头的请求都可以访问
```

斜杠和扩展名不能同时存在,下面这个例子就是不合法的   

```
/.html
```   

注册Servlet 有另外一种方式，除了web.xml配置，还可以使用注解。   
```
@WebServlet("/hello")
``` 
在待注册的Servlet加上注解，通过访问注解里的url来访问 Servlet  
在tomcat 服务器中运行的结果  
```
HelloServlet constuct
init
service
destroy  
```



# Servlet 的四大域对象

servlet 有九个内置对象，分别为：```pageContext、ServletRequest、HttpSession、ServletContext```

所有域对象都是有存取数据的功能，因为域对象内部有一个Map，用来存储数据，下面是ServletContext对象用来操作数据的方法：   

|方法	|作用|
|-|-|
|void setAttribute(String name,Object val)|	用来存储一个对象，也可以成为存储一个域，参数分别为域的属性名和属性值，如果多次调用该方法，并且使用相同的name，那么就会覆盖上一次的值，这一特性与Map相似
Object getAttribute(String name)	|用来获取ServletContext中的数据，当前在获取之前需要先存储。
void removeAttribute(String name)|	用来移除ServletContext中的域属性，如果参数name不存在，那么该方法什么都不做
Enumeration getAttributeNames()|	获取所有域属性的名称


**一、pageContext**    

pageContext 是Jsp 页面中才有的对象。

- 生命周期  
当对JSP请求时创建，当响应结束时销毁
 
- 作用范围   
整个JSP页面，是四大域中最小的一个。


- 作用
pageContext 对象封装了8大隐式对象，通过它可以获得其它的8个对象

方法|	作用
|-|-|
getException()	|返回Exception
getPage()	|返回Page
getRequest()|	返回request
getResponse()	|返回response
getServletConfig()|	返回config
getServletContext()	|返回application
getSession()|	返回session
getOut()|	返回out   

二、Servletrequest

- 生命周期   
在service方法调用前由服务器创建，传入service()方法，整个请求结束，ServletRequest生命周期结束。
- 作用范围   
仅在当前请求中有效，请求的转发也是一个请求。
- 作用   
常用语服务器间同一请求不同页面之间的参数传递，常用语表单的控件值传递。

> 扩展
HttpServletRequest:是ServletRequest 的子接口，针对于HTTP请求所定义，里面包含了大量获取HTTP请求相关的方法。
- 获取请求的URL    
HttpServletRequest.getRequestURI()
- 获取请求方式   
HttpServletRequest.getMethod()   
- 若是一个get请求，获取请求参数的字符串，若是 post 请求，则结果为null
HttpServletRequest.getQueryString
- 获取请求的Servlet 映射路径   
HttpServletRequest.getServletPath



**三、HttpSession**

- 生命周期   
服务器在运行时可以为每一个用户的浏览器创建一个其独享的session对象，由于session为用户浏览器独享，所以用户在访问服务器的web资源时，可以把各自的数据放在各自的session中，当用户再去访问服务器中的其它web资源时，其它web资源再从用户各自的session中取出数据为用户服务。
在第一次调用request.getSession()方法时，服务器会检查是否已经有对应的session，如果没有就在内存中创建一个并返回。
当一段时间内，session没有被使用（默认是30分钟），服务器会销毁该session。如果服务器非正常关闭（强行关闭），还未到期的session也会被销毁。
另外，调用session的invalidate()方法可以立即销毁session。
- 作用范围   
一次会话

- 作用       
常用于web开发中的登录验证界面（当用户登陆成功后浏览器分配其中一个Session键值对）。


**四、ServletContext**   
- 生命周期   
当web应用被加进容器时，创建代表整个web应用的ServletContext对象，当服务器关闭，或web应用被移除时，ServletContext对象跟着被销毁。
- 作用范围   
整个web应用
- 作用
    - 由于一个web应用中的所有Servlet共享同一个ServletContext对象：因此Servlet对象之间可以通过ServletContext来是实现通讯。ServletContext对象通常也被称为context域对象
    - 获取WEB应用程序的初始化参数   
    配置参数


```
<!--配置当前 web 应用的初始化参数-->
<context-param>
  <param-name>driver</param-name>
  <param-value>com.mysql.jdbc.driver</param-value>
</context-param>


<context-param>
  <param-name>jdbcUrl</param-name>
  <param-value>jdbc:mysql:///atguigu</param-value>
</context-param
```  
获初始化参数  
```
//获取ServletContext 对象
ServletContext context = servletConfig.getServletContext();


String driver = context.getInitParameter("driver");
System.out.println("driver:"+driver);


Enumeration<String> ns = context.getInitParameterNames();
while(ns.hasMoreElements()){
    String name = ns.nextElement();
    System.out.println("---->"+name);
}
```
- 记录日志
- application 域范围的属性
- 访问资源文件context.getResourceAsStream("jdbc.properties")
- 获取虚拟路径所映射的本地路径getRealPath("/note.txt")
- 获取当前WEB应用的名称getContextPath()
- WEB应用程序之间的访问
- SerevletContext的其它方法    




# Servlet 的转发和重定向

请求的转发和跳转都是为了实现页面的跳转.
**转发**
假设浏览器访问servlet1，而servlet1想让servlet2为客户端服务。此时servlet1调用forward（）方法，将请求转发给servlet2。但是调用forward()方法，对于浏览器来说是透明的，浏览器并不知道为其服务的Servlet已经换成Servlet2，它只知道发出了一个请求，获得了一个响应。浏览器URL的地址栏不变。
转发的对象是RequestDispatcher,有两种方式可以获取到这个对象

- HttpservletRequest的getRequestDispatcher()
- ServletContext 的getRequestDispatcher()

```
//1.调用HttpServletRequest 的getRequestDispatcher()获取RequestDispatcher 对象
//调用getRequestDispatcher()想要传入要转发的地址
String path = "testServlet1";
RequestDispatcher requestDispatcher = req.getRequestDispatcher("/" + path);
//2.调用HttpServletRequest 的forward(request,response)进行请求的转发
requestDispatcher.forward(req,resp);
```


**重定向**  
浏览器访问servlet1,而servlet1 想让servlet2 为客户端服务，此时servlet1 调用sendRedirect()将请求重定向到Servlet2.接着浏览器访问servlet2,servlet2 对客户段请求做出反应，浏览器的URL地址改变
```
//执行请求的重定向，直接调用response.sendRedirect(path)方法
//path 为要重定向的地址
String path = "testServlet1";
resp.sendRedirect(path);
```   
**转发和重定向的区别**

**本质区别**   
请求的转发是只发出了一次请求，而重定向则发出了两次请求。

**具体区别**   
请求转发:地址栏是初次发出请求的地址
请求重定向：地址栏不再是初次发出的请求地址地址栏为最后响应的那个地址

请求转发：在最终的Servlet 中，request 对象和中转的那个request 是同一个对象
请求重定向：在最终的Servlet 中，request 对象和中转的那个request 不是同一个对象

请求转发：只能转发给当前WEB应用的资源
请求重定向：可以重定向到任何资源

请求转发：/代表的是当前WEB应用的根目录 ```http://localhost:8080/project/```
请求重定向：/代表的是当前WEB站点的根目录 ```http://localhost:8080/```



# GenericServlet 和 HttpServlet       

**一、GenericServelt**
GenericServelt 是Servlet 接口的实现类，但它是一个抽象类   
```
public abstract class GenericServlet implements Servlet, ServletConfig, Serializable {
```  

它实现了Servlet、ServletConfig、Serializable 这几个接口。   
- ServletConfig 接口 
```
public interface ServletConfig {
    //得到Servlet的名字
    String getServletName();   
    
    //获取ServletContext对象
    ServletContext getServletContext();

    //获取初始化参数
    String getInitParameter(String var1);
    
    //获取初始化参数的Enumeration 对象
    Enumeration<String> getInitParameterNames();
}
```   
GenericServlet内部有一个私有的ServletConfig 对象。
```
private transient ServletConfig config; 
```

通过这个对象实现了ServletConfig 的方法。   
- ServletGenericServelt 实现了 Servlet 的除service()外所有的方法，GenericServelt这个抽象类只有一个抽象方法就是service()，我们继承GenericServlet 去实现Servlet 的时候只需要实现service()就可以了。

**init()和init(ServletConfig config)**
```
    //这个方法是Serevlet 接口的方法。
    public void init(ServletConfig config) throws ServletException {
        this.config = config;
        this.init();
    }

    public void init() throws ServletException {
    }
```     

init(ServletConfig coonfig) 方法是 Servlete 自带的方法，在初次访问这个方法时自动调用，我们如果想自己实现这个方法只需要实现init()方法就可以了。
如果我们覆盖了init(ServletConfig coonfig)方法，那么config 就不会被初始化，调用ServletConfig 的时候就会出现空指针异常。

  
- 自己实现的一个GenericServlet   
```
public  abstract class MyGenericServlet implements Servlet,ServletConfig{
    private ServletConfig servletConfig;

    public void init(ServletConfig servletConfig) throws ServletException {
        this.servletConfig = servletConfig;
    }

    public ServletConfig getServletConfig() {
        return servletConfig;
    }

    public abstract void service(ServletRequest servletRequest, ServletResponse servletResponse) throws ServletException, IOException;

    public String getServletInfo() {

        return null;
    }

    public void destroy() {

    }

    /**
     * 以下方法为ServletConfig 接口的方法
     * @return
     */
    public String getServletName() {
        return servletConfig.getServletName();
    }

    public ServletContext getServletContext() {
        return servletConfig.getServletContext();
    }

    public String getInitParameter(String s) {

        return servletConfig.getInitParameter(s);
    }

    public Enumeration<String> getInitParameterNames() {

        return servletConfig.getInitParameterNames();
    }
}

```   
**二、HttpServlet**   
HttpServlet 是专门针对于Http协议定义的一个Servlet基类，它也是一个抽象类，继承于GenericServlet. 
```
public abstract class HttpServlet extends GenericServlet {
```  
所以我们只需要实现Service(SrevletReeqequest,ServletResponse)方法即可。Servlet 的参数是ServletRequest和ServletResponse,但是因为所有的请求都是HTTP请求，所以传给Service 的其实是HttpServletRequst和HttpServletResponse.如果使用HttpServletRequest和HttpServletResponse那么需要进行强转。
```
 
 public void service(ServletRequest req, ServletResponse res) throws ServletException, IOException {
        if (req instanceof HttpServletRequest && res instanceof HttpServletResponse) {
            HttpServletRequest request = (HttpServletRequest)req;
            HttpServletResponse response = (HttpServletResponse)res;
            this.service(request, response);
        } else {
            throw new ServletException("non-HTTP request or response");
        }
    }

```  
在HttpServlet 类里面的service()方法进行了强转然后调用了service(HttpServletRequest, HttpServletresponse);  
```
protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        String method = req.getMethod();
        long lastModified;
        if (method.equals("GET")) {
            lastModified = this.getLastModified(req);
            if (lastModified == -1L) {
                this.doGet(req, resp);
            } else {
                long ifModifiedSince = req.getDateHeader("If-Modified-Since");
                if (ifModifiedSince < lastModified) {
                    this.maybeSetLastModified(resp, lastModified);
                    this.doGet(req, resp);
                } else {
                    resp.setStatus(304);
                }
            }
        } else if (method.equals("HEAD")) {
            lastModified = this.getLastModified(req);
            this.maybeSetLastModified(resp, lastModified);
            this.doHead(req, resp);
        } else if (method.equals("POST")) {
            this.doPost(req, resp);
        } else if (method.equals("PUT")) {
            this.doPut(req, resp);
        } else if (method.equals("DELETE")) {
            this.doDelete(req, resp);
        } else if (method.equals("OPTIONS")) {
            this.doOptions(req, resp);
        } else if (method.equals("TRACE")) {
            this.doTrace(req, resp);
        } else {
            String errMsg = lStrings.getString("http.method_not_implemented");
            Object[] errArgs = new Object[]{method};
            errMsg = MessageFormat.format(errMsg, errArgs);
            resp.sendError(501, errMsg);
        }

    }
```
 ```service() ```会根据请求的方式来调用相应的方法，HttpServlet 里面写了相应的doXXX()方法. 我们在继承HttpServlet 实现Servlet 的时候只需要覆盖```doXXX()```方法即可 .   

**Http请求方法**   
```GET``` 通过请求URI得到资源    
```POST``` 用于添加新的内容    
```PUT``` 用于修改某个内容   
```DELETE``` 删除某个内容   
```CONNECT``` 用于代理进行传输，如使用SSL    
```OPTIONS``` 询问可以执行哪些方法   
```PATCH``` 部分文档更改   
```RACE``` 用于远程诊断服务器   
```HEAD``` 类似于GET, 但是不返回body信息，用于检查对象是否存在，以及得到对象的元数据    
```TRACE``` 用于远程诊断服务器       


**自己实现一个HttpServlet**  
```
/**
 * 针对 HTTP 协议定义的一个 Servlet 基类
 */
public class MyHttpServletRequest extends MyGenericServlet {
    public void service(ServletRequest servletRequest, ServletResponse servletResponse) throws ServletException, IOException {
        if (servletRequest instanceof HttpServletRequest) {
            HttpServletRequest request = (HttpServletRequest) servletRequest;
            if (servletResponse instanceof HttpServletResponse) {
                HttpServletResponse response = (HttpServletResponse) servletResponse;
                //调用下面的service 方法
                service(request, response);
            }
        }
    }

    public void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //获取请求方式
        String method = request.getMethod();

        //根据强求方式再调用对应的处理方法
        if ("GET".equalsIgnoreCase(method)) {
            doGet(request, response);
        }
        if ("POST".equalsIgnoreCase(method)) {
            doPost(request, response);
        }
    }

    public void doPost(HttpServletRequest request, HttpServletResponse response) throws IOException {

    }

    public void doGet(HttpServletRequest request, HttpServletResponse response) {

    }
}
```   
