# 一、简介 
- 专门对其它对象身上发生的事件或状态的改变进行监听和相应处理的对象，当被监视的对象发生情况时立即采取相应的行动.
- Servlet 监听器：Servlet 规范中定义的一种特殊类，它用于监听web 应用程序中的ServletContext,HttpSession和ServletRequest 等域对象的创建与销毁事件，以及监听这些域对象中的属性发生修改的事件. 

# 二、分类
按照监听事件类型

- 监听域对象自身的创建和销毁的事件监听器
- 监听域对象中的属性的增加和删除的事件监听器
- 监听绑定到HttpSession 域中的某个对象的状态的事件监听器

域对象创建和销毁的事件监听器就是用来监听ServletContext，HttpSession，HttpServletRequest 这三个对象的创建和销毁事件的监听器.
域对象的创建和销毁的时机     

|域对象	|创建时机	|销毁时机|
|-|-|-|
|ServletContext	|web 服务器启动时为每个web应用程序创建相应的ServletContext对象|	web 服务器关闭时为每个web 应用程序销毁响应的ServletContext 对象
|HttpSession	浏|览器开始与服务器会话时创建	|调用HttpSession.invalidate()，或者超过了session的最大有效时间间隔，服务器进程被停止
|ServletRequest	|每次请求开始时创建	|每次访问结束后销毁

# 三、实现
- Servlet 规范为没追踪事件监听器都定义了相应的接口，开发人员编写的事件监听器程序只需实现这些接口，web 服务器根据用户编写的事件监听器所实现的接口把它注册到相应的被监听对象上。
- 一些Servlet 事件监听器需要在web 应用程序的web.xml文件中进行注册，一个web.xml 文件可以注册多个Servlet 事件监听器，Web服务器按照它们在web.xml 文件中的注册顺序来加载和注册这些Servlet 事件监听器
- Servlet 事件监听器的注册和调用过程都是由web容器自动完成的，当发生被监听的对象被创建，修改或者销毁事件时，web 容器将调用与之相关的Servlet 事件监听器对象的相关方法，开发人员在这些方法中编写的事件处理代码即被执行.
- 由于一个web应用程序只会为每个事件监听器创建一个对象，有可能出现多个线程同时调用同一个事件监听器对象的情况，所以，在编写事件监听器类时，应考虑多线程安全的问题.

# 四、 域对象的创建和销毁的事件监听器
**ServletContextListener**
监听ServletContext 对象被创建或销毁的Servlet 监听器

1. 创建一个实现了ServletContextListener的类，并且实现其中的两个方法    
```
public class MyServletContextListener implements ServletContextListener {
    @Override
    public void contextInitialized(ServletContextEvent servletContextEvent) {
        System.out.println("contextInitialized……");
    }

    @Override
    public void contextDestroyed(ServletContextEvent servletContextEvent) {
        System.out.println("contextDestroyed……");
    }
}
```   
2. 在web.xml文件中配置Listerer 
```
<listener>
    <listener-class>com.test.MyServletContextListener</listener-class>
  </listener>
```   
ServletContextListen 是最常用的Listener,可以在当前WEB应用被加载时对当前WEB应用的相关资源进行初始化操作. 创建数据库连接池、创建Spring的IOC容器、读取当前WEB应用的初始化参数……
API  
```
//ServletContext 对象被创建（当前WEB应用被加载时，）Servlet 容器调用该方法
contextInitialized(ServletContextEven sce)
//ServletContext 对象被销毁时（当前WEB应用被卸载时），Servlet 容器调用该方法
contextDestory(ServletContextEven sce)
```   

**HttpSessionListen**
```
  @Override
    public void sessionCreated(HttpSessionEvent httpSessionEvent) {
        System.out.println("Session 创建");
    }

    @Override
    public void sessionDestroyed(HttpSessionEvent httpSessionEvent) {
        System.out.println("Session 销毁");
    }
```

**ServletRequestListen** 
```
    @Override
    public void requestDestroyed(ServletRequestEvent servletRequestEvent) {
        System.out.println("Request 销毁");
    }

    @Override
    public void requestInitialized(ServletRequestEvent servletRequestEvent) {
        System.out.println("Request 初始化");
    }
```   















# 五、属性操作的事件监听器

ServletContextAttributeListener、HttpSessionAttributeListener、ServletRequestAttributeListener
写一个属性操作的监听器.  
```
public class TestAttributeListener implements ServletContextAttributeListener,
	ServletRequestAttributeListener, HttpSessionAttributeListener{

    <!--request 属性操作的监听器-->
	@Override
	public void attributeAdded(ServletRequestAttributeEvent srae) {
		System.out.println("向 request 中添加了一个属性: " + srae.getName() + ": " + srae.getValue());
	}

	@Override
	public void attributeRemoved(ServletRequestAttributeEvent srae) {
		System.out.println("从 request 中移除了一个属性: " + srae.getName() + ": " + srae.getValue());
	}

	@Override
	public void attributeReplaced(ServletRequestAttributeEvent srae) {
		System.out.println(("request 中属性替换了: "+ srae.getName() + ": " + srae.getValue()));
	}
    <!--ServletContext的属性操作监听器-->
	@Override
	public void attributeAdded(ServletContextAttributeEvent scab) {

	}

	@Override
	public void attributeRemoved(ServletContextAttributeEvent scab) {

	}

	@Override
	public void attributeReplaced(ServletContextAttributeEvent scab) {

	}
	
    <!--HttpSession 中的属性操作的监听器-->
	@Override
	public void attributeAdded(HttpSessionBindingEvent se) {

	}
    
	@Override
	public void attributeRemoved(HttpSessionBindingEvent se) {

	}

	@Override
	public void attributeReplaced(HttpSessionBindingEvent se) {

	}
}
```

test-attribute-listener.jsp
```
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Insert title here</title>
</head>
    <body>
    	
    	<% 
    		request.setAttribute("name", "ABCD");
    		System.out.println("-----------------------------");
    		
    		request.setAttribute("name", "DEFG");
    		System.out.println("-----------------------------");
    		
    		request.removeAttribute("name");
    		System.out.println("-----------------------------");
    	%>
    	
    </body>
</html>
```

执行结果：
>向 request 中添加了一个属性: name: ABCD   
request 中属性替换了: name: ABCD   
从 request 中移除了一个属性: name: DEFG   


ServletContext和HttpSession的属性监听器和它类似.
# 六、监听绑定到 HttpSession 域中的某个对象的状态的事件监听器  
HttpSessionBindingListener:监听实现了该接口的 Java 类的对象被绑定到 session 或从 session 中解除绑定的事件
HttpSessionActivationListener: 监听实现了该接口和 Serializable 接口的 Java 类的对象随 session 钝化和活化事件  
- 锐化：向磁盘中写入 session 对象
- 活化：从磁盘中读取 session 对象
注意：该监听器不需要在 web.xml 文件中进行配置.    
```
public class Customer implements HttpSessionBindingListener, HttpSessionActivationListener{
	private String time;
	public void setTime(String time) {
		this.time = time;
	}
	@Override
	public void valueBound(HttpSessionBindingEvent event) {
		System.out.println("绑定到 session");
		Object value = event.getValue();
		System.out.println(value == this);
		System.out.println(event.getName()); 
	}
	@Override
	public void valueUnbound(HttpSessionBindingEvent event) {
		System.out.println("从 sessoin 中解除绑定");
	}
	@Override
	public void sessionWillPassivate(HttpSessionEvent se)
	{
		System.out.println("从内存中写到磁盘上...");
	}
	@Override
	public void sessionDidActivate(HttpSessionEvent se)
	{
		System.out.println("从磁盘中读取出来...");
	}
	@Override
	public String toString() {
		return super.toString() + ", time: " + time;
	}
}
```
写一个例子实践一下:    
```
public class Customer implements HttpSessionBindingListener, HttpSessionActivationListener{
	private String time;
	public void setTime(String time) {
		this.time = time;
	}
	@Override
	public void valueBound(HttpSessionBindingEvent event) {
		System.out.println("绑定到 session");
		Object value = event.getValue();
		System.out.println(value == this);
		System.out.println(event.getName()); 
	}
	@Override
	public void valueUnbound(HttpSessionBindingEvent event) {
		System.out.println("从 sessoin 中解除绑定");
	}
	@Override
	public void sessionWillPassivate(HttpSessionEvent se)
	{
		System.out.println("从内存中写到磁盘上...");
	}
	@Override
	public void sessionDidActivate(HttpSessionEvent se)
	{
		System.out.println("从磁盘中读取出来...");
	}
	@Override
	public String toString() {
		return super.toString() + ", time: " + time;
	}
}
```

执行结果：

>绑定到 session   
创建一个新的 Customer 对象: com.test.Customer@517b372b, time: Tue Apr 21 12:27:47 CST 2020, 并放入到 session 中
从 session 中读取到 Customer 对象: com.test.Customer@517b372b, time: Tue Apr 21 12:27:47 CST 2020
----------------关闭服务器进程--------------------------------------------------------------------------------   
从内存中写到磁盘上...     
从 sessoin 中解除绑定

从执行结果来看，首先调用的方法是绑定到Session,然后关闭浏览器，调用sessionWillPassivate方法，最后解除绑定.   
