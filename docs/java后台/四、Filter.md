# Filter简介
- Filter 的基本功能是对Servlet容器调用Servlet的过程进行拦截，从而在Servlet进行响应处理的前后实现一些特殊的功能.   
- 在Servlet API 中定义了三个接口类来供开发人员编写Filter 程序：Filter,FilterChain,FilterConfig   
- Filter 程序是一个实现了Filter 接口的java类，与Servlet 程序相似，它由Servlet 容器进行调用和执行   
- Filter 程序需要在web.xml 文件中进行注册和设置它所能拦截的资源：Filter     程序可以拦截JSP,Servlet,静态图片文件和静态html 文件.   

# 一、Filter 是什么？
- Java Web 的一个重要组件，可以对发送到Servlet 的请求进行拦截并对响应也进行拦截.
- Filter 是实现了Filter 接口的java 类
- Filter 需要造web.xml文件中进行配置和映射    


# 二、如何创建一个Filter并运行    
- 创建一个Filter 类：实现Filter接口
```
public class HelloFilter implements Filter {
    /**
     * filter 创建之后立即被调用且只被调用一次
     * @param filterConfig
     * @throws ServletException
     */
    @Override
    public void init(FilterConfig filterConfig) throws ServletException {
        System.out.println("init……");
    }
    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
        System.out.println("doFilter……");
    }
    @Override
    public void destroy() {
        System.out.println("destory");
    }
}
```
- 在web.xml 文件中配置并映射该Filter
```
<!--注册Filter-->
  <filter>
    <filter-name>helloFilter</filter-name>
    <filter-class>com.jm.javaweb.HelloFilter</filter-class>
  </filter>

  <!--映射Filter-->
  <filter-mapping>
    <filter-name>helloFilter</filter-name>
    <url-pattern>/test.html</url-pattern>
  </filter-mapping>
 ```
- 这里的url-pattern 指的是拦截的资源，当访问这个url 时这个Filter 被调用.  
 
- 在同一个web.xml 文件中可以为同一个Filter 设置多个映射。若一个Filter 链中多次出现了同一个Filter 程序，则个Filter 程序的拦截处理过程将被多次执行.

# 三、Filter 相关的API    

1. Filter 接口
- init(FilterConfig filterConfig)方法
    - 类似于Servlet 的init()方法,在创建Filter对象(Filter对象在Servlet  容器加载当前WEB应用时即被创建)后立即被调用且只被调用一次.该方法用于对当前的Filter 进行初始化工作.Filter 实例是单例的.
    - FilterConfig 类似于ServletConfig
    - 可以在web.xml文件中配置当前Filter 的初始化参数，配置方式也和Servlet 类似.
- doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain)
    - 真正Filter 的逻辑代码需要编写在该方法中
    - FilterChain：多个Filter，可以构成一个Filter链
    - 这个方法是把请求传给Filter链的下一个Filter,若当前Filter是Filter 链的最后一个Filter,将把请求传给目标Servlet(JSP)
    - 多个Filter 拦截的顺序和配置的顺序有关，靠前的先被调用.
- destroy() 方法    
- 
 释放当前 Filter 所占用的资源的方法，在Filter 被销毁之前调用，且只被调用一次.
# 四、Filter 的基本工作原理
- 当在web.xml 中注册了一个Filter 来对某个Servlet程序进行拦截处理时，这个Filter 就成了Servlet容器与该Servlet 程序的通信线路上的一道关卡，该Filter 可以对Servlet 容器发送给Servlet 程序的请求和Servlet 程序回送给Servlet 容器的响应进行拦截，可以决定是否请求继续传递给Servlet程序，以及对请求和响应信息是否进行修改.
- 在一个web应用程序中可以注册多个Filter 程序，每个Filter 程序都可以对一个或一组Servlet 程序进行拦截
- 若有多个Filter 程序对某个Servlet程序的访问进行拦截，当针对该Servlet的访问请求到达时，web 容器将把这多个Filter 程序组合成一个Filter 链(过滤器链).Filter 链中各个Filter 的拦截顺序与它们在应用程序的web.xml 中的映射顺序一致
- 与开发Servlet 不同的是,Filter接口并没有相应的实现类可供继承，要开发过滤器，只能直接实现Filter 接口.
# 五、自定义一个专门处理Http的过滤器    
```
/**
 * 自定义的Http接口，实现自Filter接口
 */
public abstract class HttpFilter implements Filter {
    /**
     * 用于保存FilterConfig 对象
     */
    private FilterConfig config;

    /**
     * 不建议子类直接覆盖，若直接覆盖，
     * 将有可能会导致filterConfig 成员变量初始化失败
     * @param filterConfig
     * @throws ServletException
     */
    @Override
    public void init(FilterConfig filterConfig) throws ServletException {
        this.config = filterConfig;
        init();
    }

    /**
     * 供子类继承的初始化方法，可以通过getFilterConfig 获取FilterConfig对象
     */
    public void init() {
    }

    /**
     * 直接返回init(servletConfig) 的FilterConfig 对象
     * @return
     */
    public  FilterConfig getFilterConfig(){
        return config;
    }

    /**
     * 原生的doFilter 方法，在方法内部把ServletRequest和ServletResponse
     * 转为了HttpServeltRequest和HttpServletResponse
     * 并调用doFilter(HttpServletRequest servletRequest, HttpServletResponse servletResponse, FilterChain filterChain)
     * 若编写过滤器的过滤方法，不建议直接继承该方法，而建议继承
     * doFilter(HttpServletRequest servletRequest, HttpServletResponse servletResponse, FilterChain filterChain)
     *
     * @param servletRequest
     * @param servletResponse
     * @param filterChain
     * @throws IOException
     * @throws ServletException
     */
    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
        HttpServletRequest req = (HttpServletRequest) servletRequest;
        HttpServletResponse resp = (HttpServletResponse) servletResponse;
        doFilter(req,resp,filterChain);
    }

    /**
     * 抽象方法，为Http请求定制，必须实现的方法
     * @param req
     * @param resp
     * @param chain
     * @throws IOException
     * @throws ServletException
     */
    public abstract void doFilter(HttpServletRequest req, HttpServletResponse resp, FilterChain chain)throws IOException, ServletException;

    /**
     * 空的destory方法
     */
    @Override
    public void destroy() {

    }
}
```
# 六、过滤器链执行顺序   
定义两个Filter,再定义一个JSP页面，通过访问JSP页面看这两个过滤器的执行顺序.  
- HelloFilter  
```
public class HelloFilter implements Filter {
    /**
     * filter 创建之后立即被调用且只被调用一次
     * @param filterConfig
     * @throws ServletException
     */
    @Override
    public void init(FilterConfig filterConfig) throws ServletException {
        System.out.println("init……");
    }

    @Override
    public void doFilter(ServletRequest req, ServletResponse resp, FilterChain chain) throws IOException, ServletException {
        System.out.println("1.Before HelloFilter's chain.doFiltet");
        //放行
        chain.doFilter(req,resp);
        System.out.println("2.After HelloFilter's chain.doFilter");
    }

    @Override
    public void destroy() {
        System.out.println("destory");
    }
}
```   
- SecondFilter 
```
public class SecondFilter implements Filter {
    @Override
    public void init(FilterConfig filterConfig) throws ServletException {
        System.out.println("second init");
    }

    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
        System.out.println("3.Before SecondFilter's doFiltet");
        //放行
        filterChain.doFilter(servletRequest,servletResponse);
        System.out.println("4.After SecondFilter's doFilter");
    }

    @Override
    public void destroy() {
        System.out.println("second destory");
    }
}
  
```   
- test.jsp  
```
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
    <head>
        <title>Title</title>
    </head>
    <body>
        <h4>Test Page</h4>
        <%
            System.out.println("5.Test JSP");
        %>
    </body>
</html>
```
执行结果:
> 1.Before HelloFilter's chain.doFiltet   
3.Before SecondFilter's doFiltet   
5.Test JSP   
4.After SecondFilter's doFilter   
2.After HelloFilter's chain.doFilter 

Filter 的执行顺序和在web.xml 中的配置顺序有关， 越靠前，Filter 越先执行，在这个例子中，HelloFilter在SecondFilter之前配置，所以先执行HelloFilter。在访问JSP页面之前，请求会先经过两个Filter 处才能访问JSP页面，JSP 或者Servlet 给与响应之后响应又会再通过SecondFilter和HelloFilter在返回浏览器.
# 七、映射Filter  
元素用于设置一个Filter 所负责拦截的资源。一个Filter 拦截的资源可通过两种方式来指定：Servlet名称和资源访问的请求路径    

- 子元素用于设置filter 的注册名称。该值必须实在 元素中声明过的过滤器名字
- 设置filter 所拦截的请求路径（过滤器关联的URL样式）
- 指定过滤器所拦截的Servlet 名称
- 指定过滤器所拦截的资源被Servlet容器调用的方式，可以是REQUEST,INCLUDE,FORWARD和ERROR之一，默认REQUEST,可以设置多个子元素用来指定Filter 对资源的多种调用方式进行拦截.    
    - REQUEST:当用户直接访问页面时，Web 容器将会调用过滤器。如果目标资源时通过RequestDispatcher的include()或forward()方法访问时，通过GET或POST请求直接访问    
    - INCLUDE：如果目标资源时通过RequestDispatcher的include()方法访问时，那么该过滤器将会被调用，除此之外，该过滤器不会被调用

    ```
    <jsp:include file=""/>   
    ```         

    - FORWARD:forward()方法访问时，那么该过滤器将会被调用，除此之外，该过滤器不会被调用           
    
    ```
        <jsp:forward page=""/> 
        <%@ page errorPage=""/>
    ```
    
    - ERROR:如果目标资源时通过声明式异常处理机制调用时，那么该过滤器将会被调用，除此之外，过滤器不会被调用.
    ```
    //在web.xml 文件中通过error-page节点进行声明
    <error-page>
        <exception-type>java.lang.ArithmeticException</exception-type>
        <location>/test.jsp</location>
    </error-page>
    ```   
    
# 八、应用
1. 使浏览器不缓存页面的过滤器
- 有3个HTTP响应头字段都可以禁止浏览器缓存当前页面，它们在Servlet 中的示例代码如下：
    ```
        response.setDtaeHeader("Expires",-1);
        response.setHeader("Cache-Control","no-cache");
        response.setHeader("Pragma","no-cache");
    ```
- 并不是所有的浏览器都能完全支持上面的三个响应头，因此最好使同时使用上面的三个响应头
```
public class NoCacheFilter extends HttpFilter {
    @Override
    public void doFilter(HttpServletRequest req, HttpServletResponse resp, FilterChain chain) throws IOException, ServletException {
        System.out.println("cacheFilter's doFilter...");
        resp.setDateHeader("Expires",-1);
        resp.setHeader("Cache-Control","no-cache");
        resp.setHeader("Pragma","no-cache");
        chain.doFilter(req,resp);
    }
}
```   
2. 字符编码过滤器
通过配置参数上encoding 指明使用何种字符编码，以处理Html Form 请求参数的中文问题
```
public class EncodingFilter extends HttpFilter {
    private String encoding;
    @Override
    public void doFilter(HttpServletRequest req, HttpServletResponse resp, FilterChain chain) throws IOException, ServletException {
        System.out.println("encoding's Filter");
        encoding = getFilterConfig().getServletContext().getInitParameter("encoding");
        req.setCharacterEncoding(encoding);
        chain.doFilter(req,resp);
    }
}
```
web.xml
```
<context-param>
    <param-name>encoding</param-name>
    <param-value>UTF-8</param-value>
</context-param>

<filter>
    <filter-name>encoding</filter-name>
    <filter-class>com.jm.encoding.EncodingFilter</filter-class>
  </filter>
  <filter-mapping>
    <filter-name>encoding</filter-name>
    <url-pattern>/encoding/*</url-pattern>
  </filter-mapping>
```   
3. 检测用户是否登录的浏览器
- 情景：系统中某些页面只有在正常登录后才可以使用，用户请求这些页面时要检查session 中有无该用户信息，但在所有必要的页面加上session 的判断相当麻烦的事情.
- 解决方案：编写一个用于检测用户是否登录的过滤器，如果用户未登录，则重定向到指定的登录页面.
- 要求：需检查的在Session 中保存的关键字；如果用户未登录，需重定向到指定的页面（URL不包括ContextPath）,不做检查的URL列表（以分号分开，并且URL中不包括ContextPath）都要采取可配置的方式.   

**页面**    
- login.jsp：登录页面，请求提交到doLogin
```
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
    <html>
    <head>
        <title>Title</title>
    </head>
    <body>
        <form action="doLogin.jsp" method="post">
            username:<input type="text" name="username"/>
            <input type="submit" value="Submit"/>
        </form>
    </body>
</html>
```
- doLogin.jsp:判断用户信息，存入Session.
```
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
    <html>
    <head>
        <title>Title</title>
    </head>
    <body>
        <%
            //1.获取用户的登录信息
            String username = request.getParameter("username");
            //2.若登录信息完整，则把登录信息放到session里面
            if(username != null && !username.equals("")){
                session.setAttribute(application.getInitParameter("userSessionKey"),username);
                //3.重定向到list.jsp
                response.sendRedirect("list.jsp");
            }else{
                response.sendRedirect("login.jsp");
            }

        %>
    </body>
</html>
```
- list.jsp:list 列表，所有可以访问的页面链接.用户可不登录直接访问   
```
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
    <html>
    <head>
        <title>Title</title>
    </head>
    <body>
        <a href="a.jsp"/>AAA<br><br>
        <a href="b.jsp"/>BBB<br><br>
        <a href="c.jsp"/>CCC<br><br>
        <a href="d.jsp"/>DDD<br><br>
        <a href="e.jsp"/>EEE<br><br>
    </body>
</html>
```
- web.xml   
```
  <!--用户信息放入session中键的名字-->
  <context-param>
    <param-name>userSessionKey</param-name>
    <param-value>USERSESSIONKEY</param-value>
  </context-param>

  <!--若未登录，需重定向的页面-->
  <context-param>
    <param-name>redirectPage</param-name>
    <param-value>/login/login.jsp</param-value>
  </context-param>

  <!--不需要拦截或检查的url列表：list login a-->
  <context-param>
    <param-name>uncheckedUrls</param-name>
    <param-value>/login/a.jsp,/login/list.jsp,/login/login.jsp,/login/doLogin.jsp,/login/b.jsp</param-value>
  </context-param>

  <filter>
    <filter-name>loginFilter</filter-name>
    <filter-class>com.jm.login.LoginFilter</filter-class>
  </filter>

  <filter-mapping>
    <filter-name>loginFilter</filter-name>
    <url-pattern>/login/*</url-pattern>
  </filter-mapping>
```
- LoginFilter  
```
public class LoginFilter extends HttpFilter {
    //1.获取web.xml获取sessionkey，redirectUrl，uncheckUrls
    private String sessionKey;
    private String redirectUrl;
    private String uncheckedUrls;
    @Override
    public void init() {
        ServletContext context = getFilterConfig().getServletContext();
        //
        sessionKey = context.getInitParameter("userSessionKey");
        //重定向页面
        redirectUrl = context.getInitParameter("redirectPage");
        //不用检查的url
        uncheckedUrls = context.getInitParameter("uncheckedUrls");
    }
    @Override
    public void doFilter(HttpServletRequest req, HttpServletResponse resp, FilterChain chain) throws IOException, ServletException {
        //1.获取请求的URL
        String url = req.getRequestURL().toString();
        String uri = req.getRequestURI();
        String servletPath = req.getServletPath();
        //2.检查1获取的servletPath是否为不需要检查的url中的一个，如果是，则直接放行，方法结束
        List<String> urls = Arrays.asList(uncheckedUrls.split(","));
        if(urls.contains(servletPath)){
            chain.doFilter(req,resp);
            return;
        }
        //3.从session 中获取sessionKey 对应的值，若值不存在，则重定向到redirectUrl
        Object user = req.getSession().getAttribute(sessionKey);
        if(user == null){
            resp.sendRedirect(req.getContextPath()+redirectUrl);
        }
        //4.若存在，则放行，允许访问
        chain.doFilter(req,resp);
    }
}
```
4. 使用Filter 完成一个简单的权限模型：   
需求:

1. 管理权限
- 查看某人的权限
- 修改某人的权限

实现:
login.jsp
 ```
  <html>
    <head>
        <title>Title</title>
    </head>
    <body>
        <form action="${pageContext.request.contextPath}/loginServlet?method=login" method="post">
            name:<input type="text" name="name"/>
            <input type="submit" value="Submit"/>
        </form>
    </body>
</html>
```
authority-manager.jsp
有一个text文本框，供输入username,提交后，使用checkbox 显示当前用户所有的权限的信息.
检查request 中是否有user信息，若有，则显示xxx的权限为：对应的权限的checkbox 打上对号. 提示，页面上需要两层循环的方式来筛选出被选择的权限.
```
<html>
    <head>
        <title>Title</title>
    </head>
    <body>
        <center>
            <br><br>
            <form action="${pageContext.request.contextPath}/AuthorityServlet?method=getAuthorities" method="post">
              name:<input type="text" name="username"/>
                <input type="submit" value="Submit"/>
            </form>
            <c:if test="${requestScope.user != null}">
                ${requestScope.user.username} 的权限是:
                <br><br>
                <form action="${pageContext.request.contextPath}/AuthorityServlet?method=updateAuthority" method="post">
                    <input type="hidden" name="username" value="${requestScope.user.username}"/>
                    <c:forEach items="${authorities}" var="auth">
                        <c:set var="flag" value="false"/>
                        <c:forEach items="${user.authorities}" var="ua">
                            <c:if test="${ua.url == auth.url}">

                                <c:set var="flag" value="true"/>
                            </c:if>
                        </c:forEach>

                        <c:if test="${flag == true}">
                            <input type="checkbox" name="authority" value="${auth.url}" checked="checked">${auth.displayName}
                        </c:if>
                        <c:if test="${flag == false}">
                            <input type="checkbox" name="authority" value="${auth.url}">${auth.displayName}
                        </c:if>
                        <br><br>
                    </c:forEach>
                    <input type="submit" value="Update"/>
                </form>
        </c:if>
        </center>
     </body>
</html>
```   
2. 对访问进行权限控制：有权限则可以访问，否则提示：没有对应的权限，请返回
```
public class AuthorityFilter extends HttpFilter {
    @Override
    public void doFilter(HttpServletRequest req, HttpServletResponse resp, FilterChain chain) throws IOException, ServletException {
//        - 获取ServletPath
        String servletPath = req.getServletPath();
        //不需要被拦截的url列表
        List<String> uncheckUrls = Arrays.asList("/authority/403.jsp", "/authority/articles.jsp", "/authority/authority-manager.jsp", "/authority/login.jsp", "/authority/logout.jsp");
        if(uncheckUrls.contains(servletPath)){
            chain.doFilter(req,resp);
            return;
        }

//      在用户已经登录（可使用用户是否登录的过滤器）的情况下，获取用户信息：session.getAttribute("user");
        User user = (User) req.getSession().getAttribute("user");
        if(user == null){
              resp.sendRedirect(req.getContextPath()+"/authority/login.jsp");
        }
//      在获取用户所具有的权限的信息：List<Authority>
        List<Authority> authorities = user.getAuthorities();

//      检验用户是否有请求servletPath的权限:可以思考除了遍历之外，有没有更好的实现方式
        Authority authority = new Authority(null, servletPath);

//      若有权限则：响应(这里因为contains方法判断要使用equals,所以我们必须要重写equals方法)
        if (authorities.contains(authority)) {
            chain.doFilter(req,resp);
            return;
        }
//      若没有权限：重定向到403.jsp
        resp.sendRedirect(req.getContextPath()+"/authority/403.jsp");
    }
}

```   
- 用户若登录，需要把用户信息（User）放入到 HttpSession 中
- 在检验权限之前，需要判断用户是否已经登录.

3.管理权限
- 封装权限信息：Authority
```
public class Authority {
    //显示到页面上的权限的名字
    private String displayName;
    //权限对应的URL地址：一个权限对应一个URL   private String url;
    private String url;
    public Authority() {

    }
    public Authority(String displayName, String url) {
        this.displayName = displayName;
        this.url = url;
    }
    public String getDisplayName() {
        return displayName;
    }
    public void setDisplayName(String displayName) {
        this.displayName = displayName;
    }
    public String getUrl() {
        return url;
    }
    public void setUrl(String url) {
        this.url = url;
    }
    @Override
    public boolean equals(Object o) {
        if (this == o)
            return true;
        if (o == null || getClass() != o.getClass())
            return false;
        Authority authority = (Authority) o;
        return Objects.equals(url, authority.url);
    }
    @Override
    public int hashCode() {
        return Objects.hash(url);
    }
}
```
- 封装用户信息：User
```
public class User {
    private String username;
    private List<Authority> authorities;

    public User() {
    }

    public User(String username, List<Authority> authorities) {
        this.username = username;
        this.authorities = authorities;
    }

    public String getUsername() {
        return username;
    }

    public void setUsername(String username) {
        this.username = username;
    }

    public List<Authority> getAuthorities() {
        return authorities;
    }

    public void setAuthorities(List<Authority> authorities) {
        this.authorities = authorities;
    }
}
```   
- 创建一个UseryDao：
```
public class UserDao {
    private static Map<String,User> users;
    private static List<Authority> authorities = null;

    static {
        //用户所有的权限
        authorities = new ArrayList<Authority>();
        authorities.add(new Authority("Article-1","/authority/article-1.jsp"));
        authorities.add(new Authority("Article-2","/authority/article-2.jsp"));
        authorities.add(new Authority("Article-3","/authority/article-3.jsp"));
        authorities.add(new Authority("Article-4","/authority/article-4.jsp"));

        users = new HashMap<String,User>();
        //给AAA用户添加12权限
        User user1 = new User("AAA",authorities.subList(0,2));
        users.put("AAA", user1);
        //给BBB用户添加34权限
        user1 = new User("BBB",authorities.subList(2,4));
        users.put("BBB",user1);

    }
    //通过用户名获取用户
    public User get(String username){
        return users.get(username);
    }
    //更新用户权限
    public void update(String username,List<Authority> authorities){
        System.out.println("username:"+username);
        if(users.get(username) == null){
            System.out.println(username+"不存在");
        }
        users.get(username).setAuthorities(authorities);
    }
    //获取所有的权限信息
    public  List<Authority> getAuthorities(){
        return authorities;
    }
    public List<Authority> getAuthorities(String[] urls) {
        //新的权限集合
        List<Authority> authorities2 = new ArrayList<Authority>();
        for(Authority authority:authorities){
            if(urls != null){
                for(String url:urls){
                    if(url.equals(authority.getUrl())){
                        authorities2.add(authority);
                    }
                }
            }
        }
        return authorities2;
    }
}
```   
- Servlet    
authority-manager.jsp 提交表单后get方法:获取表单的请求参数：username,再根据username获取User 信息.把user放入到request 请求域中,转发到authority-manager.jsp.
authority-manager.jsp 修改权限的表单提交后update方法:获取请求参数：username,authority（多选）;把权限封装为List;调用UserDao 的update() 方法实现权限的修改;重定向到authority-manager.jsp     
```
public class AuthorityServlet extends HttpServlet {

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        String methodName = req.getParameter("method");
        System.out.println(methodName);
        try {
            Method method = getClass().getMethod(methodName, HttpServletRequest.class, HttpServletResponse.class);
            method.invoke(this, req, resp);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
    
    private UserDao userDao = new UserDao();
    //获取用户全选
    public void getAuthorities(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        String username = req.getParameter("username");
        User user = userDao.get(username);
        req.setAttribute("user", user);
        req.setAttribute("authorities",userDao.getAuthorities());
        req.getRequestDispatcher("/authority/authority-manager.jsp").forward(req, resp);
    }
    //更新权限
    public void updateAuthority(HttpServletRequest req,HttpServletResponse resp) throws IOException {
        String username = req.getParameter("username");
        String[] authorities = req.getParameterValues("authority");
        List<Authority> authorityList = userDao.getAuthorities(authorities);
        userDao.update(username,authorityList);
        resp.sendRedirect(req.getContextPath()+"/authority/authority-manager.jsp");
    }
}
```
  
5.为论坛过滤不雅文字和HTML特殊字符
- 开发论坛模块时要解决以下两个问题：
    - 用户回复发帖时可能会插入如HTML代码（例如：<,>等），这可能会破坏论坛的正常显示，也可能会带来安全隐患
    - 某些用户在回复时可能会输入不雅句子，这些字句会给论坛带来不好的影响
    - 实现对不雅文字的可配置
    - 要求：不雅文字及其替代内容实现可配置   
 
Serevlet API 也提供了一个HttpSerevletRequestWrapper类来包装原始的request 对象.
HttpServletRequestWrapper 类实现了HttpServletRequest 接口中的所有方法.
这些方法的内部实现都是仅仅调用了一下所包装的 request 对象的对应方法.

相类似Servlet API 也提供了一个HttpServletResponseWrapper 类来包装原始的response 对象.     

**包装类 ServletRequest 实现**
```
public class HttpServletRequestWrapper extends ServletRequestWrapper implements HttpServletRequest {
    public HttpServletRequestWrapper(HttpServletRequest request) {
        super(request);
    }

    private HttpServletRequest _getHttpServletRequest() {
        return (HttpServletRequest)super.getRequest();
    }

    public String getAuthType() {
        return this._getHttpServletRequest().getAuthType();
    }

    public Cookie[] getCookies() {
        return this._getHttpServletRequest().getCookies();
    }

    public long getDateHeader(String name) {
        return this._getHttpServletRequest().getDateHeader(name);
    }
    ……
}
```   
作用:对HttpServletRequest 或HttpServletResponse 的某一个方法进行修改或者增强
MyHttpServletRequest  
```
public class MyHttpServletRequest extends HttpServletRequestWrapper {
    public MyHttpServletRequest(HttpServletRequest request) {
        super(request);
    }
    @Override
    public String getParameter(String name) {
        String val = super.getParameter(name);
        if(val != null && val.contains(" fuck ")){
            val = val.replace("fuck","****");
        }
        return val;
    }
}
```
**使用**：在Filter ,利用HttpServletRequest 替换传入的HttpServletRequest
```
public void doFilter(HttpServletRequest req, HttpServletResponse resp, FilterChain chain) throws IOException, ServletException {
            //1.获取请求content参数的值
            System.out.println(req);
            HttpServletRequest request = new MyHttpServletRequest(req);
            String content = request.getParameter("content");
        }
        chain.doFilter(request,resp);
    }
```
此时到达Servlet 或JSP的HttpServletRequest 实际上是MyHttpServletRequest


# HttpServletRequestWrapper

1.为什么需要 HttpServletRequestWrapper
需要改变从Servlet 容器（可能是任何的Servlet容器）中传入的HttpServletRequest 对象的某个行为，该怎么办？

- 继承HttpServletRequest接口的Servlet容器的实现类，但就和具体的容器相耦合
- 提供HttpServletRequest接口的实现类，很麻烦，而且也需要和具体的容器相耦合。
- 使用装饰器设计模式
    - 提供一个类，该类实现HttpServletRequest 接口
    - 传入具体的一容器实现的HttpServletRequest 接口的实现类作为上述类的一个成员变量
    - 使用HttpServletRequest 成员变量来实现HttpServletRequest 接口的所有方法。
- 提供HttpServletRequestWrapper 的实现类，实现具体的方法.
