# Web临时笔记





##### 二、如何知晓页面的请求方法【使用httpServletRequest类向下造型】

```java
public void service(ServletRequest servletRequest, ServletResponse servletResponse) throws ServletException, IOException {

    //ServletRequest是接口， 无法直接使用该对象调用getMethod()方法，故需要他的实现类
    //向下转型
    HttpServletRequest httpServlet=(HttpServletRequest) servletRequest;
    String method = httpServlet.getMethod();
    if("GET".equals(method)){
        doGet();
    }else if("POST".equals(method)){
        doPost();
    }

}


//将get请求处理方案封装函数中
public void doGet(){
    System.out.println("当前请求方法：get");
}
//将post请求处理方案封装函数中
public void doPost(){
    System.out.println("当前请求方法：post");
}
```



##### 三、通过继承：HttpServlet实现Servlet程序【实际开发中常用方案】

（一）使用HttpServlet创建servlet程序的原因

一般在实际项目开发，都是使用继承HttpServlet类的方式去实现Servlet程序

（二）HttpServlet类

1. 属于Servlet接口的实现类

（三）通过HttpServlet实现Servlet程序流程

1. 编写一个类，继承HttpServlet类
2. 根据业务重写doGet  以及  doPost方法
3. 到web.xml文档中配置该Servlet程序的访问地址。 

（四）源码实现servlet程序

```java
package com.stu.servlet_;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

public class helloServlet02 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("get方法被访问");
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("post方法被访问");
    }
}
```

（五）通过IDEA创建Servlet更加快速的方案【默认拥有：doGet方法   doPost方法】

![image-20220109190742612](C:\Users\27932\AppData\Roaming\Typora\typora-user-images\image-20220109190742612.png)





##### 四、Servlet类的继承体系

![image-20220110093157362](C:\Users\27932\AppData\Roaming\Typora\typora-user-images\image-20220110093157362.png)

![image-20220110093307544](C:\Users\27932\AppData\Roaming\Typora\typora-user-images\image-20220110093307544.png)

**明天任务：**

1. Servlet继承体系图学习  ✔
2. ServletConfig类学习 ✔
3. ServletText类的学习
4. Http协议学习





### 三、ServletConfig类

##### 一、ServletConfig接口类介绍

1. ServletConfig类从类名上来看，就知道是Servlet程序的配置信息类。
2. Servlet程序和ServletConfig对象由Tomcat负责创建， 我们负责使用
3. Servlet程序默认是第一次访问的时候创建， ServletConfig是每个Servlet程序创建时，就创建一个对应的ServletConfig对象。 





##### 二、ServletConfig接口类三大作用

（一）可获取Servlet程序的别名值【XML文件中：servlet-name标签内容】

1. 在init方法中使用
2. 通过servletConfig引用进行

```java
public void init(ServletConfig servletConfig) throws ServletException {
    //1.通过servletConfig引用获取到servlet程序的别名
    System.out.println("servlet程序的别名："+servletConfig.getServletName());
}
```



（二）获取初始化参数innit-param【XML文件中：init-param标签内容，且前提是XML文件中有所配置】

```java
public void init(ServletConfig servletConfig) throws ServletException {
    //2.获取到初始化参数，getInitParameter("参数名")
    System.out.println("当前helloServlet程序的username初始化参为："+servletConfig.getInitParameter("username"));
}
```



（三）获取ServletContext接口类对象

```java
public void init(ServletConfig servletConfig) throws ServletException {
    //3.获取servletConText对象
    System.out.println("获取ServletConText对象"+servletConfig.getServletContext());

}
```





##### 三、不同渠道获取到ServletConfig引用以及ServletConfig的使用问题

（一）getServletConfig()方法

通过该方法可以得到一个ServletConfig引用对象。 



（二）ServletConfig的使用问题

1. 每个Servlet程序中的ServletConfig对象只能获取被程序中的配置！！！无法获取外界的servlet！
2. innit方法使用必须添加：super.init(config);

​     原因：

1. 需要使用父类的init方法才能使用config

```java
public void init(ServletConfig config) throws ServletException {
    super.init(config);
    System.out.println("helloServlet02程序的innit初始化做了些工作");
}
```





![image-20220112150153288](C:\Users\27932\AppData\Roaming\Typora\typora-user-images\image-20220112150153288.png)



获取请求方式：String getMethod()
获取虚拟目录（上下文路径）:String getContextPath()
获取Servlet路径：String getServletPath()
获取GET方式的请求参数 String getQueryString()
获取请求URI:String getRequesURI()
获取请求URL:String getRequestURL()
获取协议及版本：String getProtocol()
获取客户机的IP地址：String getRemoteAddr()
