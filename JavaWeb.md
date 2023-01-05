# 最基础的东西
## JavaWeb是什么鬼
JavaWeb： 所有通过Java写的，可通过浏览器访问的程序的总称======》<font color="red">JavaWeb是基于<font color="blue">请求响应</font>来开发的 </font>
![](/img/JavaWeb/2023-01-01-22-19-28.png)

需求定完了，代码写完了，测试测完了，然后呢？要发布了吧？  <font color="red"> ======》</font>用maven或者IDEA等工具把你的代码打成一个<font color="red">[war包](#war)</font>，然后把这个war包发布到你的生产环境下的web容器（tomcat/jboss/weblogic/websphere/jetty/resin）里<font color="red"> ======》</font>发布完了之后，你要启动你的web容器，开始提供服务<font color="red"> ======》</font>配置域名，dns等等相关，你的网站就可以访问了（假设你是个网站）<font color="red"> ======》</font>前后端代码是不是全都在那个war包里？包括你的js，css，图片，各种第三方的库，对吧？好，下面在浏览器中输入你的网站域名开始访问旅程...

### war包和jar包

<span id="war">war包</span>是用来

### JavaWeb三大组件

#### Servlet（JavaWeb的核心）

Servlet是运行在Web服务器中的小型Java程序，通过HTTP接收和响应来自Web客户端的请求。

Servlet程序3.0版本之前，市面上使用更多，主要是基于xml配置（老年人做法）；
![](/img/JavaWeb/2023-01-02-17-12-15.png)

![](/img/JavaWeb/2023-01-02-17-23-33.png)

Servlet的生命周期：执行构造器方法->init->service方法（每次访问都会执行）->destroy销毁（Web工程停止才会执行）

![](/img/JavaWeb/2023-01-02-17-28-04.png)

![](/img/JavaWeb/2023-01-02-17-28-49.png)
费了老大劲搞的一个Servlet程序其实就是为了处理一个请求响应

就算是老年人，在实际开发中也很少会像上面一样直接实现Servlet这个接口来做Servlet程序，实际中都是继承功能更加全面强大的HttpServlet类，重写它的方法来搞的

![](/img/JavaWeb/2023-01-02-17-56-14.png)

![](/img/JavaWeb/2023-01-02-18-00-45.png)

![](/img/JavaWeb/2023-01-02-18-03-53.png)

![](/img/JavaWeb/2023-01-02-18-10-00.png)

实际是这么个做法：

![](/img/JavaWeb/2023-01-02-17-41-41.png)

![](/img/JavaWeb/2023-01-02-17-45-33.png)

Servlet3.0之后，即为注解版本的Servlet使用

就算换成了注解开发，现在也没几个人使用原始的Servlet与jsp直接开发，都封装成那么方便的框架了,Servlet应用上已过时（现在几乎最少也用到Spring MVC了），但<font color="red">具体实现还是靠Servlet</font>，总有一天我们会用到它去突破一些瓶颈，掌握牢，靠谱。Servlet是一组规范,是一个只有5个方法的interface，其次SprinMVC等主流JavaMVC框架都是基于Servlet。

servlet接口定义的是一套处理网络请求的规范，所有实现servlet的类，都需要实现它那五个方法，其中最主要的是两个生命周期方法 init()和destroy()，还有一个处理请求的service()，也就是说，所有实现servlet接口的类，或者说，所有想要处理网络请求的类，都需要回答这三个问题：

你初始化时要做什么
你销毁时要做什么
你接受到请求时要做什么
这是Java给的一种规范！就像阿西莫夫的机器人三大定律、行尸走肉里Rick的那三个问题一样，规范！

servlet是一个规范，那实现了servlet的类，就能处理请求了吗？

答案是，**不能**

随便谷歌一个servlet的hello world教程，里面都会让你写一个servlet，但你从来不会在servlet中写什么监听8080端口的代码，**servlet不会直接和客户端打交道！**

那请求怎么来到servlet呢？答案是servlet容器，比如我们最常用的tomcat，同样，你可以随便谷歌一个servlet的hello world教程，里面肯定会让你把servlet部署到一个容器中，不然你的servlet压根不会起作用。

tomcat才是与客户端直接打交道的家伙，他监听了端口，请求过来后，根据url等信息，确定要将请求交给哪个servlet去处理，然后调用那个servlet的service方法，service方法返回一个response对象，tomcat再把这个response返回给客户端。

 

狭义的Servlet是指Java语言实现的一个接口，广义的Servlet是指任何实现了这个Servlet接口的类，一般情况下，人们将Servlet理解为后者。

![](/img/JavaWeb/2023-01-02-18-23-43.png)

**工作模式：**

1、客户端请求该 Servlet；

2、加载 Servlet 类到内存；

3、实例化并调用init()方法初始化该 Servlet；

4、service()（根据请求方法不同调用doGet() 或者 doPost()，此外还有doHead()、doPut()、doTrace()、doDelete()、doOptions()）；

5、destroy()；

6、加载和实例化 Servlet。这项操作一般是动态执行的。然而，Server 通常会提供一个管理的选项，用于在 Server 启动时强制装载和初始化特定的 Servlet；

7、Server 创建一个 Servlet的实例；

8、第一个客户端的请求到达 Server；

9、Server 调用 Servlet 的 init() 方法（可配置为 Server 创建 Servlet 实例时调用，在 web.xml 中 <servlet> 标签下配置 <load-on-startup> 标签，配置的值为整型，值越小 Servlet 的启动优先级越高）；

10、一个客户端的请求到达 Server；

11、Server 创建一个请求对象，处理客户端请求；

12、Server 创建一个响应对象，响应客户端请求；

13、Server 激活 Servlet 的 service() 方法，传递请求和响应对象作为参数；

14、service() 方法获得关于请求对象的信息，处理请求，访问其他资源，获得需要的信息；

15、service() 方法使用响应对象的方法，将响应传回Server，最终到达客户端。service()方法可能激活其它方法以处理请求，如 doGet() 或 doPost() 或程序员自己开发的新的方法；

16、对于更多的客户端请求，Server 创建新的请求和响应对象，仍然激活此 Servlet 的 service() 方法，将这两个对象作为参数传递给它。如此重复以上的循环，但无需再次调用 init() 方法。一般 Servlet 只初始化一次(只有一个对象)，当 Server 不再需要 Servlet 时（一般当 Server 关闭时），Server 调用 Servlet 的 destroy() 方法。

#### Filter过滤器

#### Listener监听器

## Tomcat（一个轻量级的<font color="red">Web服务器（JavaWeb容器）</font>，能解析jsp、Servlet等动态资源）

严格来说，Tomcat不能说是一个服务器，它只是一个运行在物理服务器上的Servlet容器程序，充其量只能叫一个Web服务器，还达不到应用服务器的水平

静态资源：html、js、css、mp4视频、txt、图片
动态资源：jsp页面、Servlet程序

哈哈哈，新时代的JSP叫Themyleaf，很多小公司为了追求速度，或者节省人力，就是用后端渲染，就算不用jsp，用thymeleaf,freemarker都差不多，会一点后端渲染的技术还是要的。

Tomcat和Servelt/jsp、jdk之间的版本对应
![](/img/JavaWeb/2023-01-01-22-36-27.png)

