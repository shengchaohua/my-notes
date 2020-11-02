[TOC]























# Servlet
## 介绍
Servlet是运行在服务器上的Java程序，可以接收客户端的请求并返回响应信息。

定义一个Servlet类需要实现`javax.servlet.Servlet`接口，但是实现接口比较麻烦。程序员通常会选择继承`javax.servlet.http.HttpServlet`，并重写init、service、doGet、doPost和destroy等方法。
- init: 初始化方法，只执行一次。默认为第一次调用时执行，也可以指定为在服务器第一次启动时执行。
- service: 执行实际任务的主要方法，用来处理来自客户端的请求，并把格式化的响应写回给客户端。service方法检查HTTP请求类型（GET、POST、PUT、DELETE等），并在适当的时候调用doGet、doPost、doPut，doDelete等方法。
- doGet: 处理一个来自客户端的GET请求。
- doPost: 处理一个来自客户端的POST请求。
- destroy: destroy方法只会被调用一次，在Servlet生命周期结束时被调用，用于关闭数据库等类似的清理活动。调用完之后，servlet对象等待垃圾回收。

示例代码：
```java
public class TestServlet extends HttpServlet {

    @Override
    protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        super.service(req, resp);
    }

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        super.doGet(req, resp);
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        super.doPost(req, resp);
    }
}
```

Servlet对象是单例的，多次访问使用同一个Servlet对象。Servlet对象⼀旦创建了，就会驻留在内存中，为后续的请求做服务，直到服务器关闭。

对于每次请求，服务器都会创建一个新的请求对象```req```和一个新的响应对象```resp```，将这两个对象作为参数传入service方法。

对于每个请求，服务器都会创建一个线程，如果类中存在共享资源就需要处理同步问题。


## 生命周期
Servlet 生命周期可被定义为从创建直到毁灭的整个过程，包括：
1. 加载Servlet。当Tomcat第⼀次访问Servlet的时候，Tomcat会负责创建Servlet的实例。
2. 初始化。当Servlet被实例化后，Tomcat会调⽤init()⽅法初始化这个对象。
3. 处理服务。当浏览器访问Servlet的时候，Servlet会调⽤service()⽅法处理请求。
4. 销毁。当Tomcat关闭时或者检测到Servlet要从Tomcat删除的时候会⾃动调⽤destroy()⽅法，让该实例释放掉所占的资源。⼀个Servlet如果⻓时间不被使⽤的话，也会被Tomcat⾃动销毁。
5. 卸载。当Servlet调⽤完destroy()⽅法后，等待垃圾回收。如果有需要再次使⽤这个Servlet，会重新调⽤init()⽅法进⾏初始化操作。

简单总结：只要访问Servlet，service()就会被调⽤。init()只有第⼀次访问Servlet的时候才会被调⽤。destroy()只有在Tomcat关闭的时候才会被调⽤。


如何设置一个Servlet在应用程序启动时初始化？在配置文件中的`<Servlet>`标签下配置`<load-on-startup>`，那么WEB应⽤程序在启动时，就会装载并创建该Servlet的实例对象、以及调⽤该对象的init()⽅法。

```xml
<servlet>
    <servlet-name>TestServlet</servlet-name>
    <servlet-class>cn.sch.TestServlet</servlet-class>
    <load-on-startup>1</load-on-startup>
</servlet>
<servlet-mapping>
    <servlet-name>TestServlet</servlet-name>
    <url-pattern>/test</url-pattern>
</servlet-mapping>
```

应用程序启动时初始化一个Servlet的作用？完成一些应用程序启动所需的任务，比如创建必要的数据库，完成一些特定的任务。


## 转发和重定向
转发是由服务器进⾏跳。在转发的时候，浏览器的地址栏是没有发⽣变化的。浏览器是不知道该跳转的动作，转发是对浏览器透明的。转发时仍然时当前的请求，使用当前的request和response对象。这也解释了，为什么可以使⽤request作为域对象进⾏转移参数。

重定向是由浏览器进⾏跳转的，进⾏重定向跳转的时候，浏览器的地址会发⽣变化。实现重定向的原理是由response的状态码和Location头组合实现的。重定向会建立一个新的请求对象和响应对象。

转发和重定向的写法：
- 转发：`request.getRequestDispatcher("/URI").forward(request,response)`，`/`表示应用的资源根目录。
- 重定向：`response.send("/web应用名/URI");`，`/`表示应用的webapps目录

转发和重定向能够去往的URL范围不一样。转发是服务器跳转只能去往当前web应⽤的资源，重定向是服务器跳转，可以去往任何的资源。

转发和重定传递数据的类型不同。转发的request对象可以传递各种类型的数据，包括对象；重定向只能传递字符串。

转发和重定跳转的时间不同。转发是执行到跳转语句时就会⽴刻跳转；重定向是整个⻚⾯执⾏完之后才执⾏跳转。

使用转发前通常会携带参数，比如Servlet转发到JSP页面，并对页面进行填充；重定向一般不会携带参数，只是重定向到另一个页面。


## 过滤器
Servlet提供了一个javax.servlet.Filter接口，如果一个类实现了这个接口，则把这个类称之为过滤器Filter。通过Filter技术，开发人员可以实现用户在访问某个目标资源之前，对访问的请求和响应进行拦截，简单说，就是可以实现web容器对某目标资源的访问前截获进行相关的处理，还可以在某目标资源向web容器返回响应前进行截获进行处理。

开发人员通过Filter技术，对web服务器管理的所有web资源：
- 例如JSP, Servlet, 静态图片文件或静态html文件等进行拦截，从而实现一些特殊的功能。
- 例如实现URL级别的权限访问控制、过滤敏感词汇、压缩响应信息等一些高级功能。

Filter的创建和销毁是由WEB服务器负责：
- 在应用启动的时候就进行装载Filter类（与Servlet的load-on-startup配置效果相同）。
- 容器创建好Filter对象实例后，调用init()方法。接着被Web容器保存进应用级的集合容器中去了等待着，用户访问资源。
- 当用户访问的资源正好被Filter的url-pattern拦截时，容器会取出Filter类调用doFilter方法，下次或多次访问被拦截的资源时，Web容器会直接取出指定Filter对象实例调用doFilter方法(Filter对象常驻留Web容器了)。
- 当应用服务被停止或重新装载了，则会执行Filter的destroy方法，Filter对象销毁。

举例：在后台项目中使用了过滤器，把后台管理的链接设置为`admin_category_list`格式，使用过滤器对`admin_`开头的链接进行过滤，并转发到相应的Servlet。

Filter配置如下所示：
```xml
<filter>
    <filter-name>BackServletFilter</filter-name>
    <filter-class>tmall.filter.BackServletFilter</filter-class>
</filter>
 
<filter-mapping>
    <filter-name>BackServletFilter</filter-name>
    <url-pattern>/*</url-pattern>
</filter-mapping>
```

Filter代码如下所示：
```java
public class BackServletFilter implements Filter {
    @Override
    public void doFilter(ServletRequest req, ServletResponse res, FilterChain chain)
            throws IOException, ServletException {
        HttpServletRequest request = (HttpServletRequest) req;
        HttpServletResponse response = (HttpServletResponse) res;
         
        String contextPath=request.getServletContext().getContextPath();
        String uri = request.getRequestURI();
        uri =StringUtils.remove(uri, contextPath);
        if(uri.startsWith("/admin_")){     
            String servletPath = StringUtils.substringBetween(uri,"_", "_") + "Servlet";
            String method = StringUtils.substringAfterLast(uri,"_" );
            request.setAttribute("method", method);
            req.getRequestDispatcher("/" + servletPath).forward(request, response);
            return;
        }
         
        chain.doFilter(request, response);
    }
}
```


# Mybatis
> [MyBatis常见面试题总结 - JavaGuide](https://gitee.com/SnailClimb/JavaGuide/blob/master/docs/system-design/framework/mybatis/mybatis-interview.md)

## Mybatis有哪些配置文件？
Mybatis包括两种配置文件：
- XML配置文件，全局配置文件，包括了对 MyBatis 系统的核心设置，文件名一般为`mybatis-config.xml`
- XML映射文件，一个映射文件对应一个Mapper接口，用来实现Mapper接口中的方法

XML配置文件的格式一般为：
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
  PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
  <environments default="development">
    <environment id="development">
      <transactionManager type="JDBC"/>
      <dataSource type="POOLED">
        <property name="driver" value="${driver}"/>
        <property name="url" value="${url}"/>
        <property name="username" value="${username}"/>
        <property name="password" value="${password}"/>
      </dataSource>
    </environment>
  </environments>
  <mappers>
    <mapper resource="org/mybatis/example/BlogMapper.xml"/>
  </mappers>
</configuration>
```

从XML配置文件中构建 SqlSessionFactory 的实例非常简单，建议使用类路径下的资源文件进行配置。MyBatis 包含一个名叫 Resources 的工具类，它包含一些实用方法，使得从类路径或其它位置加载资源文件更加容易。
```java
String resource = "mybatis-config.xml";
InputStream inputStream = Resources.getResourceAsStream(resource);
SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
```

XML映射文件用来实现mapper接口，只有很少的几个顶级元素，格式一般为：
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
  PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="org.mybatis.example.BlogMapper">
  <select id="selectBlog" resultType="Blog">
    select * from Blog where id = #{id}
  </select>
</mapper>
```

使用映射文件比较简单，一般为：
```java
BlogMapper mapper = session.getMapper(BlogMapper.class);
Blog blog = mapper.selectBlog(101);
```


## XML配置文件
> [Mybatis-配置](https://mybatis.org/mybatis-3/zh/configuration.html)

XML配置文件的顶层结构如下：
- configuration（配置）
    - properties（属性）
    - settings（设置）
    - typeAliases（类型别名）
    - typeHandlers（类型处理器）
    - objectFactory（对象工厂）
    - plugins（插件）
    - environments（环境配置）
        - environment（环境变量）
            - transactionManager（事务管理器）
            - dataSource（数据源）
    - databaseIdProvider（数据库厂商标识）
    - mappers（映射器）

全局配置文件的configuration下的所有配置必须按规定顺序写，不按顺序会报错，，但是可以省略某些配置。


### 属性（properties）
属性可以在外部进行配置，并可以进行动态替换。你既可以在典型的 Java 属性文件中配置这些属性，也可以在 properties 元素的子元素中设置。例如
```xml
<properties resource="org/mybatis/example/config.properties">
  <property name="username" value="dev_user"/>
  <property name="password" value="F2Fa3!33TYyg"/>
</properties>
```

设置好的属性可以在整个配置文件中用来替换需要动态配置的属性值。比如:
```xml
<dataSource type="POOLED">
  <property name="driver" value="${driver}"/>
  <property name="url" value="${url}"/>
  <property name="username" value="${username}"/>
  <property name="password" value="${password}"/>
</dataSource>
```

这个例子中的 username 和 password 将会由 properties 元素中设置的相应值来替换。 

如果一个属性在不只一个地方进行了配置，那么，MyBatis 将按照下面的顺序来加载：
- 首先读取在 properties 元素体内指定的属性。
- 然后根据 properties 元素中的 resource 属性读取类路径下属性文件，或根据 url 属性指定的路径读取属性文件，并覆盖之前读取过的同名属性。
- 最后读取作为方法参数传递的属性，并覆盖之前读取过的同名属性。

因此，通过方法参数传递的属性具有最高优先级，resource/url 属性中指定的配置文件次之，最低优先级的则是 properties 元素中指定的属性。


### 设置（settings）
这是 MyBatis 中极为重要的调整设置，它们会改变 MyBatis 的运行时行为。常用的设置包括：
- cacheEnabled：全局性地开启或关闭所有映射器配置文件中已配置的任何缓存。有效值包括true、false，默认值为true。
- lazyLoadingEnabled：延迟加载的全局开关。当开启时，所有关联对象都会延迟加载。 特定关联关系中可通过设置 fetchType 属性来覆盖该项的开关状态。有效值包括true、false，默认值为false。
- useGeneratedKeys：允许 JDBC 支持自动生成主键，需要数据库驱动支持。如果设置为 true，将强制使用自动生成主键。尽管一些数据库驱动不支持此特性，但仍可正常工作（如 Derby）。有效值包括true、false，默认值为false。
- defaultExecutorType：配置默认的执行器。SIMPLE 就是普通的执行器；REUSE 执行器会重用预处理语句（PreparedStatement）； BATCH 执行器不仅重用语句还会执行批量更新。有效值包括SIMPLE、REUSE、BATCH，默认值为SIMPLE。
- lazyLoadTriggerMethods：指定对象的哪些方法触发一次延迟加载。用逗号分隔的方法列表，有效值包括equals,clone,hashCode,toString。
- proxyFactory：指定 Mybatis 创建可延迟加载对象所用到的代理工具。有效值包括CGLIB、JAVASSIST，默认值为JAVASSIST。


### 类型别名（typeAliases）
类型别名可为 Java 类型设置一个缩写名字。 它仅用于 XML 配置，意在降低冗余的全限定类名书写。例如：
```xml
<typeAliases>
  <typeAlias alias="Author" type="domain.blog.Author"/>
  <typeAlias alias="Blog" type="domain.blog.Blog"/>
  <typeAlias alias="Comment" type="domain.blog.Comment"/>
  <typeAlias alias="Post" type="domain.blog.Post"/>
  <typeAlias alias="Section" type="domain.blog.Section"/>
  <typeAlias alias="Tag" type="domain.blog.Tag"/>
</typeAliases>
```

当这样配置时，`Blog`可以用在任何使用`domain.blog.Blog`的地方。

下面是一些为常见的 Java 类型内建的类型别名。它们都是不区分大小写的，注意，为了应对原始类型的命名重复，采取了特殊的命名风格。

|别名|映射的类型|
|---|---|
|_byte|byte|
|_long|long|
|_short|short|
|_int|int|
|_integer|int|
|_double|double|
|_float|float|
|_boolean|boolean|
|string|String|
|byte|Byte|
|long|Long|
|short|Short|
|int|Integer|
|integer|Integer|
|double|Double||
|float|Float|
|boolean|Boolean|
|date|Date|
|decimal|BigDecimal|
|bigdecimal|BigDecimal|
|object|Object|
|map|Map|
|hashmap|HashMap|
|list|List|
|arraylist|ArrayList|
|collection|Collection|
|iterator|Iterator|


### 环境配置（environments）
MyBatis 可以配置成适应多种环境，这种机制有助于将 SQL 映射应用于多种数据库之中， 现实情况下有多种理由需要这么做。例如，开发、测试和生产环境需要有不同的配置；或者想在具有相同 Schema 的多个生产数据库中使用相同的 SQL 映射。还有许多类似的使用场景。

不过要记住：尽管可以配置多个环境，但每个 SqlSessionFactory 实例只能选择一种环境。

所以，如果你想连接两个数据库，就需要创建两个 SqlSessionFactory 实例，每个数据库对应一个。而如果是三个数据库，就需要三个实例，依此类推，记起来很简单：每个数据库对应一个 SqlSessionFactory 实例。

为了指定创建哪种环境，只要将它作为可选的参数传递给 SqlSessionFactoryBuilder 即可。可以接受环境配置的两个方法签名是：
```java
SqlSessionFactory factory = new SqlSessionFactoryBuilder().build(reader, environment);
SqlSessionFactory factory = new SqlSessionFactoryBuilder().build(reader, environment, properties);
```

如果忽略了环境参数，那么将会加载默认环境，如下所示：
```java
SqlSessionFactory factory = new SqlSessionFactoryBuilder().build(reader);
SqlSessionFactory factory = new SqlSessionFactoryBuilder().build(reader, properties);
```

environments 元素定义了如何配置环境。
```xml
<environments default="development">
  <environment id="development">
    <transactionManager type="JDBC">
      <property name="..." value="..."/>
    </transactionManager>
    <dataSource type="POOLED">
      <property name="driver" value="${driver}"/>
      <property name="url" value="${url}"/>
      <property name="username" value="${username}"/>
      <property name="password" value="${password}"/>
    </dataSource>
  </environment>
</environments>
```

注意一些关键点:
- 默认使用的环境 ID（比如：default="development"）。
- 每个 environment 元素定义的环境 ID（比如：id="development"）。
- 事务管理器的配置（比如：type="JDBC"）。
- 数据源的配置（比如：type="POOLED"）。

默认环境和环境 ID 顾名思义。 环境可以随意命名，但务必保证默认的环境 ID 要匹配其中一个环境 ID。

#### 事务管理器（transactionManager）
在 MyBatis 中有两种类型的事务管理器（也就是 type="[JDBC|MANAGED]"）：
- JDBC：这个配置直接使用了 JDBC 的提交和回滚设施，它依赖从数据源获得的连接来管理事务作用域。
- MANAGED：这个配置几乎没做什么。它从不提交或回滚一个连接，而是让容器来管理事务的整个生命周期（比如 JEE 应用服务器的上下文）。 默认情况下它会关闭连接。然而一些容器并不希望连接被关闭，因此需要将 closeConnection 属性设置为 false 来阻止默认的关闭行为。

提示：如果你正在使用 Spring + MyBatis，则没有必要配置事务管理器，因为 Spring 模块会使用自带的管理器来覆盖前面的配置。


#### 数据源（dataSource）
dataSource 元素使用标准的 JDBC 数据源接口来配置 JDBC 连接对象的资源。
- 大多数 MyBatis 应用程序会按示例中的例子来配置数据源。虽然数据源配置是可选的，但如果要启用延迟加载特性，就必须配置数据源。
- 有三种内建的数据源类型（也就是 type="[UNPOOLED|POOLED|JNDI]"）。

UNPOOLED，这个数据源的实现会每次请求时打开和关闭连接。虽然有点慢，但对那些数据库连接可用性要求不高的简单应用程序来说，是一个很好的选择。性能表现则依赖于使用的数据库，对某些数据库来说，使用连接池并不重要，这个配置就很适合这种情形。UNPOOLED 类型的数据源仅仅需要配置：
- driver：这是 JDBC 驱动的 Java 类全限定名（并不是 JDBC 驱动中可能包含的数据源类）。
- url：这是数据库的 JDBC URL 地址。
- username：登录数据库的用户名。
- password：登录数据库的密码。
- defaultTransactionIsolationLevel：默认的连接事务隔离级别。

POOLED，这种数据源的实现利用“池”的概念将 JDBC 连接对象组织起来，避免了创建新的连接实例时所必需的初始化和认证时间。这种处理方式很流行，能使并发 Web 应用快速响应请求。除了上述提到 UNPOOLED 下的属性外，还有更多属性用来配置 POOLED 的数据源：
- poolMaximumActiveConnections：在任意时间可存在的活动（正在使用）连接数量，默认值：10
- poolMaximumIdleConnections：任意时间可能存在的空闲连接数。
- poolMaximumCheckoutTime：在被强制返回之前，池中连接被检出（checked out）时间，默认值：20000 毫秒（即 20 秒）
- poolTimeToWait：这是一个底层设置，如果获取连接花费了相当长的时间，连接池会打印状态日志并重新尝试获取一个连接（避免在误配置的情况下一直失败且不打印日志），默认值：20000 毫秒（即 20 秒）。

JNDI，这个数据源实现是为了能在如 EJB 或应用服务器这类容器中使用，容器可以集中或在外部配置数据源，然后放置一个 JNDI 上下文的数据源引用。这种数据源配置只需要两个属性：
- initial_context：这个属性用来在 InitialContext 中寻找上下文（即，initialContext.lookup(initial_context)）。这是个可选属性，如果忽略，那么将会直接从 InitialContext 中寻找 data_source 属性。
- data_source：这是引用数据源实例位置的上下文路径。提供了 initial_context 配置时会在其返回的上下文中进行查找，没有提供时则直接在 InitialContext 中查找。


### 映射器（mappers）
配置映射器是告诉 MyBatis 到哪里去找mapper文件，以及其中的SQL语句。在自动查找资源方面，Java 并没有提供一个很好的解决方案，所以最好的办法是直接告诉 MyBatis 到哪里去找映射文件。 你可以使用相对于类路径的资源引用，或完全限定资源定位符，或类名和包名等。例如：
```xml
<!-- 使用相对于类路径的资源引用 -->
<mappers>
  <mapper resource="org/mybatis/builder/AuthorMapper.xml"/>
  <mapper resource="org/mybatis/builder/BlogMapper.xml"/>
  <mapper resource="org/mybatis/builder/PostMapper.xml"/>
</mappers>
<!-- 使用完全限定资源定位符（URL） -->
<mappers>
  <mapper url="file:///var/mappers/AuthorMapper.xml"/>
  <mapper url="file:///var/mappers/BlogMapper.xml"/>
  <mapper url="file:///var/mappers/PostMapper.xml"/>
</mappers>
<!-- 使用映射器接口实现类的完全限定类名 -->
<mappers>
  <mapper class="org.mybatis.builder.AuthorMapper"/>
  <mapper class="org.mybatis.builder.BlogMapper"/>
  <mapper class="org.mybatis.builder.PostMapper"/>
</mappers>
<!-- 将包内的映射器接口实现全部注册为映射器 -->
<mappers>
  <package name="org.mybatis.builder"/>
</mappers>
```

这些配置会告诉 MyBatis 去哪里找映射文件，剩下的细节就应该是每个 SQL 映射文件了，也就是接下来我们要讨论的。


## XML 映射器
MyBatis 的真正强大在于它的语句映射。由于它的异常强大，映射器的 XML 文件就显得相对简单。MyBatis 致力于减少使用成本，让用户能更专注于 SQL 代码。

一个XML映射文件对应一个Mapper接口（Dao接口），Mapper接口没有实现类，映射文件中需要使用namespace标识对应的Mapper接口。
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
  PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="org.mybatis.example.BlogMapper">
  <select id="selectBlog" resultType="Blog">
    select * from Blog where id = #{id}
  </select>
</mapper>
```

Mapper文件的select/insert/update/delete都会被解析成一个MappedStatement。当调用接口方法时，接口全限名+方法名拼接字符串作为 key 值，可唯一定位一个MappedStatement。

举例，对于上面的Mapper文件，当调用`blogMapper.selectBlog(1)`时，通过接口全限名和方法名找到对应的MappedStatement，传递参数，执行，返回结果。

因为映射文件需要用Mapper接口的全限定名+方法名作为方法标识符，所以接口中的方法不能有重载。

Mapper接口的工作原理是 JDK 动态代理，Mybatis 运行时会使用 JDK 动态代理为 Dao 接口生成代理 proxy 对象，代理对象 proxy 会拦截接口方法，转而执行MappedStatement所代表的 sql，然后将 sql 执行结果返回。

SQL 映射文件只有很少的几个顶级元素（按照应被定义的顺序列出）：
- cache：该命名空间的缓存配置。
- cache-ref：引用其它命名空间的缓存配置。
- resultMap：描述如何从数据库结果集中加载对象，是最复杂也是最强大的元素。
- sql：可被其它语句引用的可重用语句块。
- insert：映射插入语句。
- update：映射更新语句。
- delete：映射删除语句。
- select：映射查询语句。


### select
查询语句是 MyBatis 中最常用的元素之一。仅仅把数据存到数据库中价值并不大，还要能重新取出来才有用，多数应用也都是查询比修改要频繁。 MyBatis 的基本原则之一是：在每个插入、更新或删除操作之间，通常会执行多个查询操作。因此，MyBatis 在查询和结果映射做了相当多的改进。一个简单查询的 select 元素是非常简单的。比如：
```xml
<select id="selectPerson" parameterType="int" resultType="hashmap">
  SELECT * FROM PERSON WHERE ID = #{id}
</select>
```

这个语句名为 selectPerson，接受一个 int（或 Integer）类型的参数，并返回一个 HashMap 类型的对象，其中的键是列名，值便是结果行中的对应值。

这相当于告诉 MyBatis 创建一个预处理语句（PreparedStatement）参数，在 JDBC 中，这样的一个参数在 SQL 中会由一个“?”来标识，并被传递到一个新的预处理语句中，就像这样：
```java
String selectPerson = "SELECT * FROM PERSON WHERE ID=?";
PreparedStatement ps = conn.prepareStatement(selectPerson);
ps.setInt(1,id);
```

使用 JDBC 就意味着使用更多的代码，以便提取结果并将它们映射到对象实例中，而这就是 MyBatis 的拿手好戏。

select 元素允许你配置很多属性来配置每条语句的行为细节。
```xml
<select
  id="selectPerson"
  parameterType="int"
  parameterMap="deprecated"
  resultType="hashmap"
  resultMap="personResultMap"
  flushCache="false"
  useCache="true"
  timeout="10"
  fetchSize="256"
  statementType="PREPARED"
  resultSetType="FORWARD_ONLY">
```

select标签的属性描述：
- id：在命名空间中唯一的标识符，可以被用来引用这条语句。
- parameterType：将会传入这条语句的参数的类全限定名或别名。这个属性是可选的，因为 MyBatis 可以通过类型处理器（TypeHandler）推断出具体传入语句的参数，默认值为未设置（unset）。
- resultType：期望从这条语句中返回结果的类全限定名或别名。 注意，如果返回的是集合，那应该设置**为集合包含的类型**，而不是集合本身的类型。 resultType 和 resultMap 之间只能同时使用一个。
- resultMap：对外部 resultMap 的命名引用。结果映射是 MyBatis 最强大的特性，如果你对其理解透彻，许多复杂的映射问题都能迎刃而解。 resultType 和 resultMap 之间只能同时使用一个。
- flushCache：将其设置为 true 后，只要语句被调用，都会导致本地缓存和二级缓存被清空，默认值：false。
- useCache：将其设置为 true 后，将会导致本条语句的结果被二级缓存缓存起来，默认值：对 select 元素为 true。
- timeout：这个设置是在抛出异常之前，驱动程序等待数据库返回请求结果的秒数。默认值为未设置（unset）（依赖数据库驱动）。
- fetchSize：这是一个给驱动的建议值，尝试让驱动程序每次批量返回的结果行数等于这个设置值。 默认值为未设置（unset）（依赖驱动）。
- statementType：可选 STATEMENT，PREPARED 或 CALLABLE。这会让 MyBatis 分别使用 Statement，PreparedStatement 或 CallableStatement，默认值：PREPARED。
- resultSetType：FORWARD_ONLY，SCROLL_SENSITIVE, SCROLL_INSENSITIVE 或 DEFAULT（等价于 unset） 中的一个，默认值为 unset （依赖数据库驱动）。
- databaseId：如果配置了数据库厂商标识（databaseIdProvider），MyBatis 会加载所有不带 databaseId 或匹配当前 databaseId 的语句；如果带和不带的语句都有，则不带的会被忽略。
- resultOrdered：这个设置仅针对嵌套结果 select 语句：如果为 true，将会假设包含了嵌套结果集或是分组，当返回一个主结果行时，就不会产生对前面结果集的引用。 这就使得在获取嵌套结果集的时候不至于内存不够用。默认值：false。
- resultSets：这个设置仅适用于多结果集的情况。它将列出语句执行后返回的结果集并赋予每个结果集一个名称，多个名称之间以逗号分隔。


### insert, update 和 delete
数据变更语句 insert，update 和 delete 的实现非常接近：
```xml
<insert
  id="insertAuthor"
  parameterType="domain.blog.Author"
  flushCache="true"
  statementType="PREPARED"
  keyProperty=""
  keyColumn=""
  useGeneratedKeys=""
  timeout="20">

<update
  id="updateAuthor"
  parameterType="domain.blog.Author"
  flushCache="true"
  statementType="PREPARED"
  timeout="20">

<delete
  id="deleteAuthor"
  parameterType="domain.blog.Author"
  flushCache="true"
  statementType="PREPARED"
  timeout="20">
```

insert, update, delete标签的属性描述：
- id：在命名空间中唯一的标识符，可以被用来引用这条语句。
- parameterType：将会传入这条语句的参数的类全限定名或别名。这个属性是可选的，因为 MyBatis 可以通过类型处理器（TypeHandler）推断出具体传入语句的参数，默认值为未设置（unset）。
- flushCache：将其设置为 true 后，只要语句被调用，都会导致本地缓存和二级缓存被清空，默认值：（对 insert、update 和 delete 语句）true。
- timeout：这个设置是在抛出异常之前，驱动程序等待数据库返回请求结果的秒数。默认值为未设置（unset）（依赖数据库驱动）。
- statementType：可选 STATEMENT，PREPARED 或 CALLABLE。这会让 MyBatis 分别使用 Statement，PreparedStatement 或 CallableStatement，默认值：PREPARED。
- useGeneratedKeys：（仅适用于 insert 和 update）这会令 MyBatis 使用 JDBC 的 getGeneratedKeys 方法来取出由数据库内部生成的主键（比如：像 MySQL 和 SQL Server 这样的关系型数据库管理系统的自动递增字段），默认值：false。
- keyProperty：（仅适用于 insert 和 update）指定能够唯一识别对象的属性，MyBatis 会使用 getGeneratedKeys 的返回值或 insert 语句的 selectKey 子元素设置它的值，默认值：未设置（unset）。如果生成列不止一个，可以用逗号分隔多个属性名称。
- keyColumn：（仅适用于 insert 和 update）设置生成键值在表中的列名，在某些数据库（像 PostgreSQL）中，当主键列不是表中的第一列的时候，是必须设置的。如果生成列不止一个，可以用逗号分隔多个属性名称。
- databaseId：如果配置了数据库厂商标识（databaseIdProvider），MyBatis 会加载所有不带 databaseId 或匹配当前 databaseId 的语句；如果带和不带的语句都有，则不带的会被忽略。

如前所述，插入语句的配置规则更加丰富，在插入语句里面有一些额外的属性和子元素用来处理主键的生成，并且提供了多种生成方式。

首先，如果你的数据库支持自动生成主键的字段（比如 MySQL 和 SQL Server），那么你可以设置 useGeneratedKeys=”true”，然后再把 keyProperty 设置为目标属性就 OK 了。例如，如果上面的 Author 表已经在 id 列上使用了自动生成，那么语句可以修改为：
```xml
<insert id="insertAuthor" useGeneratedKeys="true"
    keyProperty="id">
  insert into Author (username,password,email,bio)
  values (#{username},#{password},#{email},#{bio})
</insert>
```

如果你的数据库还支持多行插入, 你也可以传入一个 Author 数组或集合，并返回自动生成的主键。
```xml
<insert id="insertAuthor" useGeneratedKeys="true"
    keyProperty="id">
  insert into Author (username, password, email, bio) values
  <foreach item="item" collection="list" separator=",">
    (#{item.username}, #{item.password}, #{item.email}, #{item.bio})
  </foreach>
</insert>
```


### sql
这个元素可以用来定义可重用的 SQL 代码片段，以便在其它语句中使用。 参数可以静态地（在加载的时候）确定下来，并且可以在不同的 include 元素中定义不同的参数值。比如：
```xml
<sql id="userColumns"> ${alias}.id,${alias}.username,${alias}.password </sql>
```

这个 SQL 片段可以在其它语句中使用，例如：
```xml
<select id="selectUsers" resultType="map">
  select
    <include refid="userColumns"><property name="alias" value="t1"/></include>,
    <include refid="userColumns"><property name="alias" value="t2"/></include>
  from some_table t1
    cross join some_table t2
</select>
```


### 参数
参数是 MyBatis 非常强大的元素。对于大多数简单的使用场景，你都不需要使用复杂的参数，比如：
```xml
<select id="selectUsers" resultType="User">
  select id, username, password
  from users
  where id = #{id}
</select>
```

原始类型或简单数据类型（比如 Integer 和 String）因为没有其它属性，会用它们的值来作为参数。然而，如果传入一个复杂的对象，行为就会有点不一样了。比如：
```xml
<insert id="insertUser" parameterType="User">
  insert into users (id, username, password)
  values (#{id}, #{username}, #{password})
</insert>
```

如果 User 类型的参数对象传递到了语句中，会查找 id、username 和 password 属性，然后将它们的值传入预处理语句的参数中。

另外，参数也可以指定一个特殊的数据类型。
```xml
#{property,javaType=int,jdbcType=NUMERIC}
```

要更进一步地自定义类型处理方式，可以指定一个特殊的类型处理器类（或别名），比如：
```xml
#{age,javaType=int,jdbcType=NUMERIC,typeHandler=MyTypeHandler}
```

参数的配置好像越来越繁琐了，但实际上，很少需要如此繁琐的配置。

#### 字符串替换
默认情况下，使用`#{}`参数语法时，MyBatis会创建`PreparedStatement`参数占位符，并通过占位符安全地设置参数（就像使用`?`一样）。这样做更安全，更迅速，通常也是首选做法，不过有时你就是想直接在 SQL 语句中直接插入一个不转义的字符串。 比如 ORDER BY 子句，这时候你可以：
```xml
ORDER BY ${columnName}
```

这样，MyBatis 就不会修改或转义该字符串了。

当 SQL 语句中的元数据（如表名或列名）是动态生成的时候，字符串替换将会非常有用。 举个例子，如果你想 select 一个表任意一列的数据时，不需要这样写：
```java
@Select("select * from user where id = #{id}")
User findById(@Param("id") long id);

@Select("select * from user where name = #{name}")
User findByName(@Param("name") String name);

@Select("select * from user where email = #{email}")
User findByEmail(@Param("email") String email);
```

而是可以只写这样一个方法：
```java
@Select("select * from user where ${column} = #{value}")
User findByColumn(@Param("column") String column, @Param("value") String value);
其中 ${column} 会被直接替换，而 #{value} 会使用 ? 预处理。 这样，就能完成同样的任务：
User userOfId1 = userMapper.findByColumn("id", 1L);
User userOfNameKid = userMapper.findByColumn("name", "kid");
User userOfEmail = userMapper.findByColumn("email", "noone@nowhere.com");
```

这种方式也同样适用于替换表名的情况。

提示：用这种方式接受用户的输入，并用作语句参数是不安全的，会导致潜在的 SQL 注入攻击。因此，要么不允许用户输入这些字段，要么自行转义并检验这些参数。


### 结果映射
`resultMap`元素是 MyBatis 中最重要最强大的元素。它可以让你从 90% 的 JDBC`ResultSets` 数据提取代码中解放出来。实际上，在为一些比如连接的复杂语句编写映射代码的时候，一份`resultMap` 能够代替实现同等功能的数千行代码。`ResultMap`的设计思想是，对简单的语句做到零配置，对于复杂一点的语句，只需要描述语句之间的关系就行了。

之前你已经见过简单映射语句的示例，它们没有显式指定`resultMap`。比如
```xml
<select id="selectUsers" resultType="map">
  select id, username, hashedPassword
  from some_table
  where id = #{id}
</select>
```

上述语句只是简单地将所有的列映射到 HashMap 的键上，这由 resultType 属性指定。虽然在大部分情况下都够用，但是 HashMap 并不是一个很好的领域模型。你的程序更可能会使用 JavaBean 或 POJO（Plain Old Java Objects，普通老式 Java 对象）作为领域模型。MyBatis 对两者都提供了支持。看看下面这个 JavaBean：
```java
package com.someapp.model;
public class User {
  private int id;
  private String username;
  private String hashedPassword;

  public int getId() {
    return id;
  }
  public void setId(int id) {
    this.id = id;
  }
  public String getUsername() {
    return username;
  }
  public void setUsername(String username) {
    this.username = username;
  }
  public String getHashedPassword() {
    return hashedPassword;
  }
  public void setHashedPassword(String hashedPassword) {
    this.hashedPassword = hashedPassword;
  }
}
```

基于 JavaBean 的规范，上面这个类有 3 个属性：id，username 和 hashedPassword。这些属性会对应到 select 语句中的列名。

这样的一个 JavaBean 可以被映射到 ResultSet，就像映射到 HashMap 一样简单。
```xml
<select id="selectUsers" resultType="com.someapp.model.User">
  select id, username, hashedPassword
  from some_table
  where id = #{id}
</select>
```

类型别名是你的好帮手。使用它们，你就可以不用输入类的全限定名了。比如：
```
<!-- mybatis-config.xml 中 -->
<typeAlias type="com.someapp.model.User" alias="User"/>

<!-- SQL 映射 XML 中 -->
<select id="selectUsers" resultType="User">
  select id, username, hashedPassword
  from some_table
  where id = #{id}
</select>
```

在这些情况下，MyBatis 会在幕后自动创建一个`ResultMap`，再根据属性名来映射列到 JavaBean 的属性上。如果列名和属性名不能匹配上，可以在 SELECT 语句中设置列别名（这是一个基本的 SQL 特性）来完成匹配。比如：
```xml
<select id="selectUsers" resultType="User">
  select
    user_id             as "id",
    user_name           as "userName",
    hashed_password     as "hashedPassword"
  from some_table
  where id = #{id}
</select>
```

在学习了上面的知识后，你会发现上面的例子没有一个需要显式配置`ResultMap`，这就是`ResultMap`的优秀之处——你完全可以不用显式地配置它们。 虽然上面的例子不用显式配置`ResultMap`。 但为了讲解，我们来看看如果在刚刚的示例中，显式使用外部的`resultMap` 会怎样，这也是解决列名不匹配的另外一种方式。
```
<resultMap id="userResultMap" type="User">
  <id property="id" column="user_id" />
  <result property="username" column="user_name"/>
  <result property="password" column="hashed_password"/>
</resultMap>
```

然后在引用它的语句中设置 resultMap 属性就行了（注意我们去掉了 resultType 属性）。比如:
```
<select id="selectUsers" resultMap="userResultMap">
  select user_id, user_name, hashed_password
  from some_table
  where id = #{id}
</select>
```

简单总结：
- 对象映射包含三种类型：默认，别名和resultMap映射。
- 如果实体类的属性字段和数据库的列名相同，只需要设置返回类型即可，Mybatis会自动设置同名的字段。一般字段不相同，可以使用sql的别名功能，将别名设置为实体类中的对应字段名。
- 最常用的是resultMap，对实体类中的字段和数据库中的列进行映射，方便重用。有了列名与属性名的映射关系后，Mybatis通过反射创建对象，同时使用反射给对象的属性逐一赋值并返回，那些找不到映射关系的属性，是无法完成赋值的。


#### 结果映射（resultMap）
resultMap标签的属性包括：
- constructor：用于在实例化类时，注入结果到构造方法中
    - idArg：ID 参数；标记出作为 ID 的结果可以帮助提高整体性能
    - arg：将被注入到构造方法的一个普通结果
- id：一个 ID 结果；标记出作为 ID 的结果可以帮助提高整体性能
- result：注入到字段或 JavaBean 属性的普通结果
- association：一个复杂类型的关联；许多结果将包装成这种类型
    - 嵌套结果映射：关联可以是 resultMap 元素，或是对其它结果映射的引用
- collection：一个复杂类型的集合
    - 嵌套结果映射：集合可以是 resultMap 元素，或是对其它结果映射的引用
- discriminator：使用结果值来决定使用哪个 resultMap
    - case：基于某些值的结果映射
        - 嵌套结果映射：case 也是一个结果映射，因此具有相同的结构和元素；或者引用其它的结果映射

最好逐步建立结果映射。单元测试可以在这个过程中起到很大帮助。 如果你尝试一次性创建像上面示例那么巨大的结果映射，不仅容易出错，难度也会直线上升。 所以，从最简单的形态开始，逐步迭代。


### 缓存
MyBatis 内置了一个强大的事务性查询缓存机制，它可以非常方便地配置和定制。 为了使它更加强大而且易于配置，我们对 MyBatis 3 中的缓存实现进行了许多改进。

默认情况下，只启用了本地的一级缓存（sqlSession会话缓存），它仅仅对一个会话中的数据进行缓存。在操作数据库时需要构造sqlSession对象，在对象中有一个数据结构（HashMap）用于存储缓存数据。不同的sqlSession之间的缓存数据区域（HashMap）是互相不影响的。

要启用全局的二级缓存，只需要在你的 SQL 映射文件中添加一行：
```
<cache/>
```

这个简单语句的效果如下:
- 映射语句文件中的所有 select 语句的结果将会被缓存。
- 映射语句文件中的所有 insert、update 和 delete 语句会刷新缓存。
- 缓存会使用最近最少使用算法（LRU, Least Recently Used）算法来清除不需要的缓存。
- 缓存不会定时进行刷新（也就是说，没有刷新间隔）。
- 缓存会保存列表或对象（无论查询方法返回哪种）的 1024 个引用。
- 缓存会被视为读/写缓存，这意味着获取到的对象并不是共享的，可以安全地被调用者修改，而不干扰其他调用者或线程所做的潜在修改。

二级缓存是mapper级别的缓存，多个SqlSession去操作同一个Mapper的sql语句，可以共用二级缓存，所以二级缓存是跨SqlSession的。

另外，二级缓存需要提交事务，才会生效。因为二级缓存用于全局共享的数据，还是在操作数据的事务结束后再生效为好。

cache 元素可以设置一些属性。比如：
```xml
<cache
  eviction="FIFO"
  flushInterval="60000"
  size="512"
  readOnly="true"/>
```

这个更高级的配置创建了一个 FIFO 缓存，每隔 60 秒刷新，最多可以存储结果对象或列表的 512 个引用，而且返回的对象被认为是只读的，因此对它们进行修改可能会在不同线程中的调用者产生冲突。

可用的清除策略有：
- LRU：最近最少使用：移除最长时间不被使用的对象。默认的清除策略。
- FIFO：先进先出：按对象进入缓存的顺序来移除它们。
- SOFT：软引用：基于垃圾回收器状态和软引用规则移除对象。
- WEAK：弱引用：更积极地基于垃圾收集器状态和弱引用规则移除对象。

flushInterval（刷新间隔）属性可以被设置为任意的正整数，设置的值应该是一个以毫秒为单位的合理时间量。 默认情况是不设置，也就是没有刷新间隔，缓存仅仅会在调用语句时刷新。

size（引用数目）属性可以被设置为任意正整数，要注意欲缓存对象的大小和运行环境中可用的内存资源。默认值是 1024。

readOnly（只读）属性可以被设置为 true 或 false。只读的缓存会给所有调用者返回缓存对象的相同实例。 因此这些对象不能被修改。这就提供了可观的性能提升。而可读写的缓存会（通过序列化）返回缓存对象的拷贝。 速度上会慢一些，但是更安全，因此默认值是 false。



## 动态SQL 
动态 SQL 是 MyBatis 的强大特性之一。动态 SQL 可以让我们在 Xml 映射文件内，以标签的形式编写动态 SQL，完成逻辑判断和动态拼接的功能。

MyBatis 3 替换了之前的大部分元素，大大精简了元素种类，现在要学习的元素种类比原来的一半还要少，包括：
- if
- choose (when, otherwise)
- trim (where, set)
- foreach


## Java API
> [Java API-Mybatis](https://mybatis.org/mybatis-3/zh/java-api.html)

==TODO==


## 各种问题
### #{}和${}的区别是什么？
${}是静态文本替换，它可以用于标签属性值和 sql 内部。比如${driver}会被静态替换为com.mysql.jdbc.Driver。

#{}是 sql 的参数占位符，Mybatis 会将 sql 中的#{}替换为?号，在 sql 执行前会使用 PreparedStatement 的参数设置方法，按序给 sql 的?号占位符设置参数值


### Executor 执行器？
Mybatis 有三种基本的 Executor 执行器，SimpleExecutor、ReuseExecutor、BatchExecutor。

- **SimpleExecutor**：每执行一次 update 或 select语句，就开启一个 Statement 对象，用完立刻关闭 Statement 对象。
- **ReuseExecutor**：可重复使用的 Statement 对象。执行 update 或 select 语句，以 sql 作为 key 查找 Statement 对象，存在就使用，不存在就创建，用完后保存起来，供下一次使用。
- **BatchExecutor**：批处理的 Statement 对象。执行 update 语句，将所有 sql 都添加到批处理中（addBatch()），等待统一执行（executeBatch()），它缓存了多个 Statement 对象，每个 Statement 对象都是 addBatch()完毕后，等待逐一执行 executeBatch()批处理。与 JDBC 批处理相同。


### Mybatis 是如何进行分页的？分页插件的原理是什么？
Mybatis 使用 RowBounds 对象进行分页，它是针对 ResultSet 结果集执行的内存分页，而非物理分页，可以在 sql 内直接书写带有物理分页的参数来完成物理分页功能，也可以使用分页插件来完成物理分页。


### Mybatis 是否支持延迟加载？如何设置？它的实现原理是什么？
延迟加载的意思是，在关联查询时，利用延迟加载，先加载主信息。需要关联信息时再去按需加载关联信息。这样会大大提高数据库性能，因为查询单表要比关联查询多张表速度要快。

Mybatis 支持 association 关联对象和 collection 关联集合对象的延迟加载，association 指的就是一对一，collection 指的就是一对多查询。

Mybatis默认是不开启延迟加载功能的，需要手动开启。在 Mybatis 配置文件中，可以配置启用延迟加载`lazyLoadingEnabled=true`，默认为false。

它的原理是，使用 CGLIB 创建目标对象的代理对象，当调用目标方法时，进入拦截器方法，比如调用 a.getB().getName()，拦截器 invoke()方法发现 a.getB()是 null 值，那么就会单独发送事先保存好的查询关联 B 对象的 sql，把 B 查询上来，然后调用 a.setB(b)，于是 a 的对象 b 属性就有值了，接着完成 a.getB().getName()方法的调用。


### 为什么说 Mybatis 是半自动 ORM 映射工具？它与全自动的区别在哪里？
Hibernate 属于全自动 ORM 映射工具，使用 Hibernate 查询关联对象或者关联集合对象时，可以根据对象关系模型直接获取，所以它是全自动的。而 Mybatis 在查询关联对象或关联集合对象时，需要手动编写 sql 来完成，所以，称之为半自动 ORM 映射工具。


### Mybatis如何实现分页？
> [MyBatis分页](https://www.jianshu.com/p/5a6bb8d0456d)

分页可以分为逻辑分页和物理分页。逻辑分页是我们的程序在显示每页的数据时，首先查询得到表中的1000条数据，然后成熟根据当前页的“页码”选出其中想要的数据来显示。

物理分页是程序先判断出该选出这1000条的第几条到第几条，然后数据库根据程序给出的信息查询出程序需要的100条返回给我们的程序。

分页的方法有多种：
1. 自定义一个PageHelper类，实现物理分页。
2. 使用RowBounds。物理分页，把数据全部查询出来，然后再根据offset和limit查询对应的记录。
3. 使用开源的PageHelper。



# Spring
## 介绍
Spring 是一种轻量级开发框架，旨在提高开发人员的开发效率以及系统的可维护性。Spring 官网：https://spring.io/。

我们一般说 Spring 框架指的都是 Spring Framework，它是很多模块的集合，使用这些模块可以很方便地协助我们进行开发。这些模块是：核心容器、数据访问/集成,、Web、AOP（面向切面编程）、工具、消息和测试模块。比如：Core Container 中的 Core 组件是Spring 所有组件的核心，Beans 组件和 Context 组件是实现IOC和依赖注入的基础，AOP组件用来实现面向切面编程。

Spring 官网列出的 Spring 的 6 个特征:
- 核心技术 ：依赖注入(DI)，AOP，事件(events)，资源，i18n，验证，数据绑定，类型转换，SpEL。
- 测试 ：模拟对象，TestContext框架，Spring MVC 测试，WebTestClient。
- 数据访问 ：事务，DAO支持，JDBC，ORM，编组XML。
- Web支持 : Spring MVC和Spring WebFlux Web框架。
- 集成 ：远程处理，JMS，JCA，JMX，电子邮件，任务，调度，缓存。
- 语言 ：Kotlin，Groovy，动态语言。


下图对应的是 Spring4.x 版本。目前最新的5.x版本中 Web 模块的 Portlet 组件已经被废弃掉，同时增加了用于异步响应式处理的 WebFlux 组件。
- Spring Core： 基础,可以说 Spring 其他所有的功能都需要依赖于该类库。主要提供 IoC 依赖注入功能。
- Spring Aspects ： 该模块为与AspectJ的集成提供支持。
- Spring AOP ：提供了面向切面的编程实现。
- Spring JDBC : Java数据库连接。
- Spring JMS ：Java消息服务。
- Spring ORM : 用于支持Hibernate等ORM工具。
- Spring Web : 为创建Web应用程序提供支持。
- Spring Test : 提供了对 JUnit 和 TestNG 测试的支持


### IOC
IoC（Inverse of Control:控制反转）是一种设计思想，就是 将原本在程序中手动创建对象的控制权，交由Spring框架来管理。 IoC 在其他语言中也有应用，并非 Spring 特有。 IoC 容器是 Spring 用来实现 IoC 的载体， IoC 容器实际上就是个Map（key，value）,Map 中存放的是各种对象。

将对象之间的相互依赖关系交给 IoC 容器来管理，并由 IoC 容器完成对象的注入。这样可以很大程度上简化应用的开发，把应用从复杂的依赖关系中解放出来。 IoC 容器就像是一个工厂一样，当我们需要创建一个对象的时候，只需要配置好配置文件/注解即可，完全不用考虑对象是如何被创建出来的。 在实际项目中一个 Service 类可能有几百甚至上千个类作为它的底层，假如我们需要实例化这个 Service，你可能要每次都要搞清这个 Service 所有底层类的构造函数，这可能会把人逼疯。如果利用 IoC 的话，你只需要配置好，然后在需要的地方引用就行了，这大大增加了项目的可维护性且降低了开发难度。

Spring 时代我们一般通过 XML 文件来配置 Bean，后来开发人员觉得 XML 文件来配置不太好，于是 SpringBoot 注解配置就慢慢开始流行起来。


### AOP
AOP(Aspect-Oriented Programming:面向切面编程)能够将那些与业务无关，却为业务模块所共同调用的逻辑或责任（例如事务处理、日志管理、权限控制等）封装起来，便于减少系统的重复代码，降低模块间的耦合度，并有利于未来的可拓展性和可维护性。

Spring AOP就是基于动态代理的，如果要代理的对象，实现了某个接口，那么Spring AOP会使用JDK Proxy，去创建代理对象，而对于没有实现接口的对象，就无法使用 JDK Proxy 去进行代理了，这时候Spring AOP会使用Cglib ，这时候Spring AOP会使用 Cglib 生成一个被代理对象的子类来作为代理，如下图所示：

使用 AOP 之后我们可以把一些通用功能抽象出来，在需要用到的地方直接使用即可，这样大大简化了代码量。我们需要增加新功能时也方便，这样也提高了系统扩展性。日志功能、事务管理等等场景都用到了 AOP 。


## Spring bean
### 作用域
- singleton : 唯一 bean 实例，Spring 中的 bean 默认都是单例的。
- prototype : 每次请求都会创建一个新的 bean 实例。
- request : 每一次HTTP请求都会产生一个新的bean，该bean仅在当前HTTP request内有效。
- session : 每一次HTTP请求都会产生一个新的 bean，该bean仅在当前 HTTP session 内有效。
- global-session： 全局session作用域，仅仅在基于portlet的web应用中才有意义，Spring5已经没有了。Portlet是能够生成语义代码(例如：HTML)片段的小型Java Web插件。它们基于portlet容器，可以像servlet一样处理HTTP请求。但是，与 servlet 不同，每个 portlet 都有不同的会话


## Spring事务
Spring 管理事务的方式分为：
- 编程式事务，在代码中硬编码。(不推荐使用)
- 声明式事务，在配置文件中配置（推荐使用）

声明式事务又分为两种：基于XML的声明式事务、基于注解的声明式事务。

### Spring 事务中的隔离级别有哪几种?
TransactionDefinition接口中定义了五个表示隔离级别的常量：
- TransactionDefinition.ISOLATION_DEFAULT：使用后端数据库默认的隔离级别，Mysql 默认采用的 REPEATABLE_READ隔离级别 Oracle 默认采用的 READ_COMMITTED隔离级别.
- TransactionDefinition.ISOLATION_READ_UNCOMMITTED：最低的隔离级别，允许读取尚未提交的数据变更，可能会导致脏读、幻读或不可重复读
- TransactionDefinition.ISOLATION_READ_COMMITTED：允许读取并发事务已经提交的数据，可以阻止脏读，但是幻读或不可重复读仍有可能发生
- TransactionDefinition.ISOLATION_REPEATABLE_READ：对同一字段的多次读取结果都是一致的，除非数据是被本身事务自己所修改，可以阻止脏读和不可重复读，但幻读仍有可能发生。
- TransactionDefinition.ISOLATION_SERIALIZABLE：最高的隔离级别，完全服从ACID的隔离级别。所有的事务依次逐个执行，这样事务之间就完全不可能产生干扰，也就是说，该级别可以防止脏读、不可重复读以及幻读。但是这将严重影响程序的性能。通常情况下也不会用到该级别。


### Spring 事务中哪几种事务传播行为?
支持当前事务的情况：
TransactionDefinition.PROPAGATION_REQUIRED： 如果当前存在事务，则加入该事务；如果当前没有事务，则创建一个新的事务。
TransactionDefinition.PROPAGATION_SUPPORTS： 如果当前存在事务，则加入该事务；如果当前没有事务，则以非事务的方式继续运行。
TransactionDefinition.PROPAGATION_MANDATORY： 如果当前存在事务，则加入该事务；如果当前没有事务，则抛出异常。（mandatory：强制性）


## 拦截器
 拦截器(Interceptor): 用于在某个方法被访问之前进行拦截，然后在方法执行之前或之后加入某些操作，其实就是AOP的一种实现策略。它通过动态拦截Action调用的对象，允许开发者定义在一个action执行的前后执行的代码，也可以在一个action执行前阻止其执行。同时也是提供了一种可以提取action中可重用的部分的方式。

过滤器和拦截器主要区别如下：
1. 二者适用范围不同。Filter是Servlet规范规定的，只能用于Web程序中，而拦截器既可以用于Web程序，也可以用于Application、Swing程序中。
2. 规范不同。Filter是在Servlet规范定义的，是Servlet容器支持的，而拦截器是在Spring容器内的，是Spring框架支持的。
3. 使用的资源不同。同其他代码块一样，拦截器也是一个Spring的组件，归Spring管理，配置在Spring文件中，因此能使用Spring里的任何资源、对象(各种bean)，而Filter不行。
4. 深度不同。Filter只在Servlet前后起作用，而拦截器能够深入到方法前后、异常跑出前后等，拦截器的使用有更大的弹性。
