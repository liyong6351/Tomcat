# 概览

Servlet是Java实现服务的基础，Tomcat和Jetty就是Servlet容器+Http服务器

## 1 Servlet接口

Servlet定义了以下接口

```
// 初始化Servlet
void init(ServletConfig var1) throws ServletException;
// 获取Servlet的配置信息
ServletConfig getServletConfig();
//调用servlet的服务接口
void service(ServletRequest var1, ServletResponse var2) throws ServletException, IOException;
//获取Servlet的详情
String getServletInfo();
//Servlet的注销接口
void destroy();
```

其中最重要的是service()方法  
ServletRequest用来封装Http的请求参数  
ServletResponse用来封装Http的响应信息  
本质上来说:上述两个参数就是对通信协议的封装

针对Http协议对应的请求参数和返回参数就是  

1. HttpServletRequest
2. HttpServletResponse

Session就是使用HttpServletRequest来创建和获取的

## 2 Servlet容器

Http服务器不直接调用Servlet，而是将请求交给Servlet容器来解决  
下面的内容将讲解Servlet容器的处理流程

```
1. 当客户端请求某个资源时，Http服务器会将请求封装成一个ServletRequest对象  
2. Servlet容器拿到请求后，根据请求的UR和Servlet映射关系找到相应的Servlet
3. 如果Servlet还没有被加载，那么这个时候回通过反射机制加载这个Servlet，并且执行init方法
4. 调用Servle的service方法来处理请求，并将ServletResponse对象返回给Http服务器
```

## 3 Web 应用

Servlet容器会实例化和调用Servlet,那么Servlet是如何注册到Servlet容器中的呢?

```
根据Servlet规范，Web应用程序目录分别放置了Servlet的类文件、配置文件和静态资源，  
Servlet通过读取配置文件，就能够加载到Servlet  
| -  MyWebApp
      | -  WEB-INF/web.xml        -- 配置文件，用来配置 Servlet 等
      | -  WEB-INF/lib/           -- 存放 Web 应用所需各种 JAR 包
      | -  WEB-INF/classes/       -- 存放你的应用类，比如 Servlet 类
      | -  META-INF/              -- 目录存放工程的一些信息
```

Servlet规范定义了ServletContext这个接口来对应Web应用，可以把ServletContext定义为一个  
全局对象，可以通过ServletContext实现Servlet之间的数据共享，也可以通过ServletContext实现  
请求转发

## 4 扩展机制

### 4.1 Filter

队请求和响应做统一的处理，比如编码、验证是否登录等  
在容器中主要是通过使用Filter链进行U形调用

### 4.2 Listener

Listener是一种监听器，当Web应用在Servlet容器中运行时，会出现很多事件，比如应用启动、  
停止或者用户请求到达等，这时候可以配置Listerner，当事件满足条件时完成某项工作