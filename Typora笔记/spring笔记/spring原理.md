## 第一章、SpringMVC常用注解

### **1、声明bean的注解**

@Component 组件，没有明确的角色

@Service 在业务逻辑层使用（service层）

@Repository 在数据访问层使用（dao层）

@Controller 在展现层使用，控制器的声明（C）

### **2、注入bean的注解**

@Autowired：由Spring提供

@Inject：由JSR-330提供

@Resource：由JSR-250提供

都可以注解在set方法和属性上，推荐注解在属性上（一目了然，少写代码）。

### **3、java配置类相关注解**

@Configuration 声明当前类为配置类，相当于xml形式的Spring配置（类上）

@Bean 注解在方法上，声明当前方法的返回值为一个bean，替代xml中的方式（方法上）

@Configuration 声明当前类为配置类，其中内部组合了@Component注解，表明这个类是一个bean（类上）

@ComponentScan 用于对Component进行扫描，相当于xml中的（类上）

@WishlyConfiguration 为@Configuration与@ComponentScan的组合注解，可以替代这两个注解

### **4、切面（AOP）相关注解**

Spring支持AspectJ的注解式切面编程。

@Aspect 声明一个切面（类上）
使用@After、@Before、@Around定义建言（advice），可直接将拦截规则（切点）作为参数。

@After 在方法执行之后执行（方法上）
@Before 在方法执行之前执行（方法上）
@Around 在方法执行之前与之后执行（方法上）

@PointCut 声明切点
在java配置类中使用@EnableAspectJAutoProxy注解开启Spring对AspectJ代理的支持（类上）

### **5、@Bean的属性支持**

@Scope 设置Spring容器如何新建Bean实例（方法上，得有@Bean）
其设置类型包括：

Singleton （单例,一个Spring容器中只有一个bean实例，默认模式）,
Protetype （每次调用新建一个bean）,
Request （web项目中，给每个http request新建一个bean）,
Session （web项目中，给每个http session新建一个bean）,
GlobalSession（给每一个 global http session新建一个Bean实例）

@StepScope 在Spring Batch中还有涉及

@PostConstruct 由JSR-250提供，在构造函数执行完之后执行，等价于xml配置文件中bean的initMethod

@PreDestory 由JSR-250提供，在Bean销毁之前执行，等价于xml配置文件中bean的destroyMethod

### **6、@Value注解**

@Value 为属性注入值（属性上）
支持如下方式的注入：
》注入普通字符

![img](https://mmbiz.qpic.cn/mmbiz_png/SJm51egHPPGzYlBica1Yoh04yOwkJRVW36yOic9uJp73qy4EJ14hJFoeNR7uTYkXc5D9zW5xq2gqguaFV8jRuZJw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

》注入操作系统属性

![img](https://mmbiz.qpic.cn/mmbiz_png/SJm51egHPPGzYlBica1Yoh04yOwkJRVW3TaL5IQCBlVHH1TtTIwEj18yia0oB1j6xf01Icicvq5iaBiaTFicQVloibg9g/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

》注入表达式结果

![img](https://mmbiz.qpic.cn/mmbiz_png/SJm51egHPPGzYlBica1Yoh04yOwkJRVW3ZZeAGw9fKhqicOpnibeh7sw2rCiaoic4PIh0qeCDY4hJg0n34aPiaIaKiaEw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

》注入其它bean属性

![img](https://mmbiz.qpic.cn/mmbiz_png/SJm51egHPPGzYlBica1Yoh04yOwkJRVW33FxlpUsmticMraMr8Nnz6LVobQThzEuoGMcRLqu4WeJEDEaicClYvRFQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

》注入文件资源

![img](https://mmbiz.qpic.cn/mmbiz_png/SJm51egHPPGzYlBica1Yoh04yOwkJRVW3aa0FXzyzD5UZHFqFibAg7AsyzwWFAT1QbUx76EnbBdPfmZyObkEXtJw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

》注入网站资源

![img](https://mmbiz.qpic.cn/mmbiz_png/SJm51egHPPGzYlBica1Yoh04yOwkJRVW3wvwkdc7BGics3Joka0zmljoVzHKcoc6tN4CnSU2U3icJzqHBLXz5ORtg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

》注入配置文件

![img](https://mmbiz.qpic.cn/mmbiz_png/SJm51egHPPGzYlBica1Yoh04yOwkJRVW3XV4ljmctSfniaP7XWRhPOuTBIZnLSo8iaGKsLpMauNNUMoZRia51TBGUw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

注入配置使用方法：
① 编写配置文件（test.properties）

book.name=《三体》

② @PropertySource 加载配置文件(类上)

![img](https://mmbiz.qpic.cn/mmbiz_png/SJm51egHPPGzYlBica1Yoh04yOwkJRVW3QxbL1MJh9Z0rJ3F43HX6cEiatZyUtGLfphbEyb1WLTtOfGHLGeiaicrHg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

③ 还需配置一个PropertySourcesPlaceholderConfigurer的bean。

### **7、环境切换**

@Profile 通过设定Environment的ActiveProfiles来设定当前context需要使用的配置环境。（类或方法上）

@Conditional Spring4中可以使用此注解定义条件话的bean，通过实现Condition接口，并重写matches方法，从而决定该bean是否被实例化。（方法上）

### **8、异步相关**

@EnableAsync 配置类中，通过此注解开启对异步任务的支持，叙事性AsyncConfigurer接口（类上）

@Async 在实际执行的bean方法使用该注解来申明其是一个异步任务（方法上或类上所有的方法都将异步，需要@EnableAsync开启异步任务）

### **9、定时任务相关**

@EnableScheduling 在配置类上使用，开启计划任务的支持（类上）

@Scheduled 来申明这是一个任务，包括cron,fixDelay,fixRate等类型（方法上，需先开启计划任务的支持）

### **10、@Enable\*注解说明**

这些注解主要用来开启对xxx的支持。
@EnableAspectJAutoProxy 开启对AspectJ自动代理的支持

@EnableAsync 开启异步方法的支持

@EnableScheduling 开启计划任务的支持

@EnableWebMvc 开启Web MVC的配置支持

@EnableConfigurationProperties 开启对@ConfigurationProperties注解配置Bean的支持

@EnableJpaRepositories 开启对SpringData JPA Repository的支持

@EnableTransactionManagement 开启注解式事务的支持

@EnableTransactionManagement 开启注解式事务的支持

@EnableCaching 开启注解式的缓存支持

### **11、测试相关注解**

@RunWith 运行器，Spring中通常用于对JUnit的支持

![img](https://mmbiz.qpic.cn/mmbiz_png/SJm51egHPPGzYlBica1Yoh04yOwkJRVW3ADU8oTU2jfpWibDicRFicJqEJX1ibjJadicX6YLBspyicCOphWZCxVbF6fBg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

@ContextConfiguration 用来加载配置ApplicationContext，其中classes属性用来加载配置类

![img](https://mmbiz.qpic.cn/mmbiz_png/SJm51egHPPGzYlBica1Yoh04yOwkJRVW3F7cIEbccnmklHSUh8wnLkC0eqY2aKf6QhKa7lKsSu3gTibv8GLPJ8vQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

SpringMVC部分



@EnableWebMvc 在配置类中开启Web MVC的配置支持，如一些ViewResolver或者MessageConverter等，若无此句，重写WebMvcConfigurerAdapter方法（用于对SpringMVC的配置）。

@Controller 声明该类为SpringMVC中的Controller

@RequestMapping 用于映射Web请求，包括访问路径和参数（类或方法上）

@ResponseBody 支持将返回值放在response内，而不是一个页面，通常用户返回json数据（返回值旁或方法上）

@RequestBody 允许request的参数在request体中，而不是在直接连接在地址后面。（放在参数前）

@PathVariable 用于接收路径参数，比如@RequestMapping(“/hello/{name}”)申明的路径，将注解放在参数中前，即可获取该值，通常作为Restful的接口实现方法。

@RestController 该注解为一个组合注解，相当于@Controller和@ResponseBody的组合，注解在类上，意味着，该Controller的所有方法都默认加上了@ResponseBody。

@ControllerAdvice 通过该注解，我们可以将对于控制器的全局配置放置在同一个位置，注解了@Controller的类的方法可使用@ExceptionHandler、@InitBinder、@ModelAttribute注解到方法上，
这对所有注解了 @RequestMapping的控制器内的方法有效。

@ExceptionHandler 用于全局处理控制器里的异常

@InitBinder 用来设置WebDataBinder，WebDataBinder用来自动绑定前台请求参数到Model中。

@ModelAttribute 本来的作用是绑定键值对到Model里，在@ControllerAdvice中是让全局的@RequestMapping都能获得在此处设置的键值对。

### 12、跨域相关

在方法上添加@CrossOrigin注解

```java
@CrossOrigin
@GetMapping("/{id}")
public Account retrieve(@PathVariable Long id) {
        // ...
}
```



## 第二章、自己手写一个SpringMVC框架

## 一、了解SpringMVC运行流程及九大组件

###    1、SpringMVC的运行流程

​       ![img](https://mmbiz.qpic.cn/mmbiz_png/jjf2cajLWRA5xCEKS8mLEMnC5S7PE73h2q7TPfX91d9ibOzX07cNr1HEbMs2IHdKjAeg1iaUYE8re6NnRpLcmhRA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

​    ⑴ 用户发送请求至前端控制器DispatcherServlet

​    ⑵ DispatcherServlet收到请求调用HandlerMapping处理器映射器。

​    ⑶ 处理器映射器根据请求url找到具体的处理器，生成处理器对象及处理器拦截器(如果有则生成)一并返回给DispatcherServlet。

​    ⑷ DispatcherServlet通过HandlerAdapter处理器适配器调用处理器

​    ⑸ 执行处理器(Controller，也叫后端控制器)。

​    ⑹ Controller执行完成返回ModelAndView

​    ⑺ HandlerAdapter将controller执行结果ModelAndView返回给DispatcherServlet

​    ⑻ DispatcherServlet将ModelAndView传给ViewReslover视图解析器

​    ⑼ ViewReslover解析后返回具体View

​    ⑽ DispatcherServlet对View进行渲染视图（即将模型数据填充至视图中）。

​    ⑾ DispatcherServlet响应用户。

​    从上面可以看出，DispatcherServlet有接收请求，响应结果，转发等作用。有了DispatcherServlet之后，可以减少组件之间的耦合度。



###     2、SpringMVC的九大组件（ref：【SpringMVC】9大组件概览）

```java
protected void initStrategies(ApplicationContext context) {
	//用于处理上传请求。处理方法是将普通的request包装成MultipartHttpServletRequest，后者可以直接调用getFile方法获取File.
	initMultipartResolver(context);
	//SpringMVC主要有两个地方用到了Locale：一是ViewResolver视图解析的时候；二是用到国际化资源或者主题的时候。
	initLocaleResolver(context); 
	//用于解析主题。SpringMVC中一个主题对应一个properties文件，里面存放着跟当前主题相关的所有资源、
	//如图片、css样式等。SpringMVC的主题也支持国际化， 
	initThemeResolver(context);
	//用来查找Handler的。
	initHandlerMappings(context);
	//从名字上看，它就是一个适配器。Servlet需要的处理方法的结构却是固定的，都是以request和response为参数的方法。
	//如何让固定的Servlet处理方法调用灵活的Handler来进行处理呢？这就是HandlerAdapter要做的事情
	initHandlerAdapters(context);
	//其它组件都是用来干活的。在干活的过程中难免会出现问题，出问题后怎么办呢？
	//这就需要有一个专门的角色对异常情况进行处理，在SpringMVC中就是HandlerExceptionResolver。
	initHandlerExceptionResolvers(context);
	//有的Handler处理完后并没有设置View也没有设置ViewName，这时就需要从request获取ViewName了，
	//如何从request中获取ViewName就是RequestToViewNameTranslator要做的事情了。
	initRequestToViewNameTranslator(context);
	//ViewResolver用来将String类型的视图名和Locale解析为View类型的视图。
	//View是用来渲染页面的，也就是将程序返回的参数填入模板里，生成html（也可能是其它类型）文件。
	initViewResolvers(context);
	//用来管理FlashMap的，FlashMap主要用在redirect重定向中传递参数。
	initFlashMapManager(context); 
}
```



## 二、梳理SpringMVC的设计思路

​    本文只实现自己的@Controller、@RequestMapping、@RequestParam注解起作用，其余SpringMVC功能读者可以尝试自己实现。



####     1、读取配置

​     ![img](https://mmbiz.qpic.cn/mmbiz_png/jjf2cajLWRA5xCEKS8mLEMnC5S7PE73hE7FnDmFWIpJY8W1ePL1qKJEpiaRBOrJkibeEzhp8robUKcBTyzJcWrRQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

  从图中可以看出，SpringMVC本质上是一个Servlet,这个 Servlet 继承自 HttpServlet。FrameworkServlet负责初始化SpringMVC的容器，并将Spring容器设置为父容器。因为本文只是实现SpringMVC，对于Spring容器不做过多讲解（有兴趣同学可以看看我另一篇文章：向spring大佬低头--大量源码流出解析）。

​    为了读取web.xml中的配置，我们用到ServletConfig这个类，它代表当前Servlet在web.xml中的配置信息。通过web.xml中加载我们自己写的MyDispatcherServlet和读取配置文件。



####     2、初始化阶段

​    在前面我们提到DispatcherServlet的initStrategies方法会初始化9大组件，但是这里将实现一些SpringMVC的最基本的组件而不是全部，按顺序包括：

- 加载配置文件
- 扫描用户配置包下面所有的类
- 拿到扫描到的类，通过反射机制，实例化。并且放到ioc容器中(Map的键值对 beanName-bean) beanName默认是首字母小写
- 初始化HandlerMapping，这里其实就是把url和method对应起来放在一个k-v的Map中,在运行阶段取出



####     3、运行阶段

​    每一次请求将会调用doGet或doPost方法，所以统一运行阶段都放在doDispatch方法里处理，它会根据url请求去HandlerMapping中匹配到对应的Method，然后利用反射机制调用Controller中的url对应的方法，并得到结果返回。按顺序包括以下功能：

- 异常的拦截
- 获取请求传入的参数并处理参数
- 通过初始化好的handlerMapping中拿出url对应的方法名，反射调用



###  三、实现自己的SpringMVC框架

​    工程文件及目录：

​              ![img](https://mmbiz.qpic.cn/mmbiz_png/jjf2cajLWRA5xCEKS8mLEMnC5S7PE73h9Zb0d9E9gYRZWwUTkhjgQw1l90JaZqGdQQmkUCMZns4jbLRSnntvag/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

首先，新建一个maven项目，在pom.xml中导入以下依赖：

```java
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <groupId>com.liugh</groupId>
  <artifactId>liughMVC</artifactId>
  <version>0.0.1-SNAPSHOT</version>
  <packaging>war</packaging>
  
	<properties>
		<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
		<maven.compiler.source>1.8</maven.compiler.source>
		<maven.compiler.target>1.8</maven.compiler.target>
		<java.version>1.8</java.version>
	</properties>
	
	<dependencies>
	     <dependency>
  		   <groupId>javax.servlet</groupId> 
		   <artifactId>javax.servlet-api</artifactId> 
		   <version>3.0.1</version> 
		   <scope>provided</scope>
		</dependency>
    </dependencies>
</project>
```

接着，我们在WEB-INF下创建一个web.xml，如下配置：

```java
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns="http://java.sun.com/xml/ns/javaee" xmlns:web="http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd"
	xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd"
	version="3.0">
	<servlet>
		<servlet-name>MySpringMVC</servlet-name>
		<servlet-class>com.liugh.servlet.MyDispatcherServlet</servlet-class>
		<init-param>
			<param-name>contextConfigLocation</param-name>
			<param-value>application.properties</param-value>
		</init-param>
		<load-on-startup>1</load-on-startup>
	</servlet>
	<servlet-mapping>
		<servlet-name>MySpringMVC</servlet-name>
		<url-pattern>/*</url-pattern>
	</servlet-mapping>

</web-app>
```

application.properties文件中只是配置要扫描的包到SpringMVC容器中。

```
scanPackage=com.liugh.core
```

创建自己的Controller注解，它只能标注在类上面：

```java
package com.liugh.annotation;

import java.lang.annotation.Documented;
import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;

@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface MyController {
	/**
     * 表示给controller注册别名
     * @return
     */
    String value() default "";

}
```

RequestMapping注解，可以在类和方法上：

```java
package com.liugh.annotation;

import java.lang.annotation.Documented;
import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;

@Target({ElementType.TYPE,ElementType.METHOD})
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface MyRequestMapping {
	/**
     * 表示访问该方法的url
     * @return
     */
    String value() default "";

}
```

RequestParam注解,只能注解在参数上

```java
package com.liugh.annotation;

import java.lang.annotation.Documented;
import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;

@Target(ElementType.PARAMETER)
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface MyRequestParam {
	/**
     * 表示参数的别名，必填
     * @return
     */
    String value();

}
```

然后创建MyDispatcherServlet这个类，去继承HttpServlet，重写init方法、doGet、doPost方法，以及加上我们第二步分析时要实现的功能：

```java
package com.liugh.servlet;

import java.io.File;
import java.io.IOException;
import java.io.InputStream;
import java.lang.reflect.Method;
import java.net.URL;
import java.util.ArrayList;
import java.util.Arrays;
import java.util.HashMap;
import java.util.List;
import java.util.Map;
import java.util.Map.Entry;
import java.util.Properties;

import javax.servlet.ServletConfig;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import com.liugh.annotation.MyController;
import com.liugh.annotation.MyRequestMapping;

public class MyDispatcherServlet extends HttpServlet{
	
	private Properties properties = new Properties();
	
	private List<String> classNames = new ArrayList<>();
	
	private Map<String, Object> ioc = new HashMap<>();
	
	private Map<String, Method> handlerMapping = new  HashMap<>();
	
	private Map<String, Object> controllerMap  =new HashMap<>();
	

	@Override
	public void init(ServletConfig config) throws ServletException {
		
		//1.加载配置文件
		doLoadConfig(config.getInitParameter("contextConfigLocation"));
		
		//2.初始化所有相关联的类,扫描用户设定的包下面所有的类
		doScanner(properties.getProperty("scanPackage"));
		
		//3.拿到扫描到的类,通过反射机制,实例化,并且放到ioc容器中(k-v  beanName-bean) beanName默认是首字母小写
		doInstance();
		
		//4.初始化HandlerMapping(将url和method对应上)
		initHandlerMapping();
		
		
	}
	
	

	@Override
	protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
		this.doPost(req,resp);
	}

	@Override
	protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
		try {
			//处理请求
			doDispatch(req,resp);
		} catch (Exception e) {
			resp.getWriter().write("500!! Server Exception");
		}

	}
	
	
	private void doDispatch(HttpServletRequest req, HttpServletResponse resp) throws Exception {
		if(handlerMapping.isEmpty()){
			return;
		}
		
		String url =req.getRequestURI();
		String contextPath = req.getContextPath();
		
		url=url.replace(contextPath, "").replaceAll("/+", "/");
		
		if(!this.handlerMapping.containsKey(url)){
			resp.getWriter().write("404 NOT FOUND!");
			return;
		}
		
		Method method =this.handlerMapping.get(url);
		
		//获取方法的参数列表
		Class<?>[] parameterTypes = method.getParameterTypes();
	
		//获取请求的参数
		Map<String, String[]> parameterMap = req.getParameterMap();
		
		//保存参数值
		Object [] paramValues= new Object[parameterTypes.length];
		
		//方法的参数列表
        for (int i = 0; i<parameterTypes.length; i++){  
            //根据参数名称，做某些处理  
            String requestParam = parameterTypes[i].getSimpleName();  
            
            
            if (requestParam.equals("HttpServletRequest")){  
                //参数类型已明确，这边强转类型  
            	paramValues[i]=req;
                continue;  
            }  
            if (requestParam.equals("HttpServletResponse")){  
            	paramValues[i]=resp;
                continue;  
            }
            if(requestParam.equals("String")){
            	for (Entry<String, String[]> param : parameterMap.entrySet()) {
         			String value =Arrays.toString(param.getValue()).replaceAll("\\[|\\]", "").replaceAll(",\\s", ",");
         			paramValues[i]=value;
         		}
            }
        }  
		//利用反射机制来调用
		try {
			method.invoke(this.controllerMap.get(url), paramValues);//第一个参数是method所对应的实例 在ioc容器中
		} catch (Exception e) {
			e.printStackTrace();
		}
	}



	private void  doLoadConfig(String location){
		//把web.xml中的contextConfigLocation对应value值的文件加载到流里面
		InputStream resourceAsStream = this.getClass().getClassLoader().getResourceAsStream(location);
		try {
			//用Properties文件加载文件里的内容
			properties.load(resourceAsStream);
		} catch (IOException e) {
			e.printStackTrace();
		}finally {
			//关流
			if(null!=resourceAsStream){
				try {
					resourceAsStream.close();
				} catch (IOException e) {
					e.printStackTrace();
				}
			}
		}
		
	}
	
	private void doScanner(String packageName) {
		//把所有的.替换成/
		URL url  =this.getClass().getClassLoader().getResource("/"+packageName.replaceAll("\\.", "/"));
		File dir = new File(url.getFile());
		for (File file : dir.listFiles()) {
			if(file.isDirectory()){
				//递归读取包
				doScanner(packageName+"."+file.getName());
			}else{
				String className =packageName +"." +file.getName().replace(".class", "");
				classNames.add(className);
			}
		}
	}
	
	
	
	private void doInstance() {
		if (classNames.isEmpty()) {
			return;
		}	
		for (String className : classNames) {
			try {
				//把类搞出来,反射来实例化(只有加@MyController需要实例化)
				Class<?> clazz =Class.forName(className);
			   if(clazz.isAnnotationPresent(MyController.class)){
					ioc.put(toLowerFirstWord(clazz.getSimpleName()),clazz.newInstance());
				}else{
					continue;
				}
				
				
			} catch (Exception e) {
				e.printStackTrace();
				continue;
			}
		}
	}


	private void initHandlerMapping(){
		if(ioc.isEmpty()){
			return;
		}
		try {
			for (Entry<String, Object> entry: ioc.entrySet()) {
				Class<? extends Object> clazz = entry.getValue().getClass();
				if(!clazz.isAnnotationPresent(MyController.class)){
					continue;
				}
				
				//拼url时,是controller头的url拼上方法上的url
				String baseUrl ="";
				if(clazz.isAnnotationPresent(MyRequestMapping.class)){
					MyRequestMapping annotation = clazz.getAnnotation(MyRequestMapping.class);
					baseUrl=annotation.value();
				}
				Method[] methods = clazz.getMethods();
				for (Method method : methods) {
					if(!method.isAnnotationPresent(MyRequestMapping.class)){
						continue;
					}
					MyRequestMapping annotation = method.getAnnotation(MyRequestMapping.class);
					String url = annotation.value();
					
					url =(baseUrl+"/"+url).replaceAll("/+", "/");
					handlerMapping.put(url,method);
					controllerMap.put(url,clazz.newInstance());
					System.out.println(url+","+method);
				}
				
			}
			
		} catch (Exception e) {
			e.printStackTrace();
		}
		
	}


	/**
	 * 把字符串的首字母小写
	 * @param name
	 * @return
	 */
	private String toLowerFirstWord(String name){
		char[] charArray = name.toCharArray();
		charArray[0] += 32;
		return String.valueOf(charArray);
	}
	
		
}
```

这里我们就开发完了自己的SpringMVC，现在我们测试一下：

```java
package com.liugh.core.controller;

import java.io.IOException;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import com.liugh.annotation.MyController;
import com.liugh.annotation.MyRequestMapping;
import com.liugh.annotation.MyRequestParam;

@MyController
@MyRequestMapping("/test")
public class TestController {
	

	
	 @MyRequestMapping("/doTest")
    public void test1(HttpServletRequest request, HttpServletResponse response,
    		@MyRequestParam("param") String param){
 		System.out.println(param);
	    try {
            response.getWriter().write( "doTest method success! param:"+param);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
	 
	 
	 @MyRequestMapping("/doTest2")
    public void test2(HttpServletRequest request, HttpServletResponse response){
        try {
            response.getWriter().println("doTest2 method success!");
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

访问http://localhost:8080/liughMVC/test/doTest?param=liugh如下：

![img](https://mmbiz.qpic.cn/mmbiz_png/jjf2cajLWRA5xCEKS8mLEMnC5S7PE73hEcDgtoJMJqvK2L6ibHJa4fzACRcD6qVkP6X7icYRRZhicq55ViawSQEicXw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

访问一个不存在的试试：

![img](https://mmbiz.qpic.cn/mmbiz_png/jjf2cajLWRA5xCEKS8mLEMnC5S7PE73hfGyeNvInCZDRT8aoJM9HFX9VcdUPAicwRiaskzYcWyiaiaU2AvypjMSlQA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

到这里我们就大功告成了！

源码地址：https://gitee.com/timoyuji/liughMVC.git

谢谢支持啦 ✧(≖ ◡ ≖✿)~ ，赞赏是对小天最棒的支持~ 星星之火可以燎原

这么良心的公众号希望大家分享给身边的小伙伴。



## 第三章、Spring Boot底层源码剖析

### 一、什么是SPI机制？

一种程序的扩展机制

![无标题](C:\Users\ly138\Desktop\无标题.jpg)

使用步骤：

1.新建com.bjpowernode.api包

2.包下新建Phone接口

3.新建两个实现类，HuaweiPhone，XiaomiPhone

4.在resources下新建**WETA-INF**文件

5.在目录下新建services文件夹

6.services文件夹下新建包名加接口名的文件：com.bjpowernode.api.Phone

7.文件中填入实现类

```
com.bjpowernode.api.HuaweiPhone
com.bjpowernode.api.XiaomiPhone
```

8.获取方法

```java
//传统
Phone huawei = new HuaweiPhone();
huawei.call();

//使用SPI
ServiceLoader<Phone> serviceLoader = ServiceLoader.load(Phone.class);
ServiceLoader.forEach(t - >{
    if(t instanceof HuaweiPhone){
        t.call();
    }
})
```

提高maven编译级别

```xml
<properties>
     <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
     <java.version>1.8</java.version>
     <spring-cloud.version>Hoxton.SR8</spring-cloud.version>
     <maven.compiler.source>1.8</maven.compiler.source>
     <maven.compiler.target>1.8</maven.compiler.target>
</properties>
```

### 二、SPI机制在开源项目中的应用

原来的项目名：java-spi

```xml
<groupId>com.bjpowernode</groupId>
<artifactId>java-spi</artifactId>
<version>1.0.0</version>
```

扩展项目名：java-spi-ext

```xml
<dependencies>  
	<groupId>com.bjpowernode</groupId>
	<artifactId>java-spi</artifactId>
	<version>1.0.0</version>
</dependencies>
```







## 第四章、自定义注解

### 一、注解的功能

定义：注解（Annotation）,也叫元数据，是一种代码级别的说明，与类、接口，枚举是在同一个层次。它可以声明在包、类、属性、方法、局部变量、方法参数等的前面，用来对这些元素进行说明，注释，用于补偿或扩展作用，

作用分类：

1.编写文档，通过代码里表示的元数据生成文档【生成文档doc文档】

2.代码分析，通过代码里标识的元数据代码进行分析【使用反射】

3.编译检查：通过代码里标识的元数据让编译器能够实现基本的编译检查【Override】

很多框架技术的新版本中，都可以使用注解替代XML配合文件

一般注解所注释的元素（类，接口，属性方法等）没有具体的影响，，仍然可以编译及执行，比如Override注解。

### 二、注解声明

声明形式，默认public，只能是public

```java
public @interface MyAnnotation {
	注解属性
}
```

注解属性看起来像个方法，其实是属性，属性类型包括所有基本类型、String、Class、enum、Annotation、以上类型的数组形式，注解元素声明形式如下：

```java
public String urlpattern();
public boolean onload();
```

修改注解类型MyAnnotation3.java为MyAnnotation4.java，添加连个属性，如下所示

```java
@Target(value = ElementType.TYPE)
@Retention(value=RetentionPolicy.RUNTIME)
public @interface MyAnnotation4{
    public String urlpattern();
	public boolean onload();
}
```

### 三、**@Target** 注解修饰目标 

 用于描述注解的范围，即注解在哪用。它说明了Annotation所修饰的对象范围：Annotation可被用于 packages、types（类、接口、枚举、Annotation类型）、类型成员（方法、构造方法、成员变量、枚举值）、方法参数和本地变量（如循环变量、catch参数）等。取值类型（ElementType） 

注解可用于不同的目标，例如杰阔，类，构造方法，方法，属性，类型等；

声明注解时，可以使用JDK已经定义的注解@Target声明注解修饰的目标；

@Target中使用ElementType表示修饰目标，有如下几种修饰目标：

| 方法            | 方法描述                           |
| --------------- | ---------------------------------- |
| *TYPE*          | 表示该注解能用于类、接口、枚举声明 |
| *FIELD*         | 表示该注解能用于域（属性）         |
| *METHOD*        | 表示该方法能用于方法               |
| PARAMETER       | 表示该注解用于方法参数             |
| CONSTRUCTOR     | 表示该方法能用于构造方法           |
| LOCAL_VARIABLE  | 表示该注解能用于局部变量           |
| ANNOTATION_TYPE | 表示该注解能用于注解类型           |
| PACKAGE         | 表示该注解能用于包                 |

代码示例  

```java
@Target({ElementType.METHOD,ElementType.CONSTRUCTOR})
public @interface Greeting {
    
}
//上述表示Log注解可以用在类、接口、enum和方法上
```

### 四、 **@Retention** 注解声明周期

JDK中已经定义了注解@Retention（保留，持久），用来定义注解的生命周期

@Retention使用枚举RetentionPolicy定义声明周期，共有三种情况

| 方法    | 方法描述                                     |
| ------- | -------------------------------------------- |
| SOURCE  | 在源文件中有效（即源文件保留）               |
| CLASS   | 在class文件中有效（即class保留）             |
| RUNTIME | 在运行时有效（即运行时保留，在java虚拟机中） |

代码示例  

```java
@Documented 
@Retention(RetentionPolicy.RUNTIME)
@Target({ElementType.METHOD,ElementType.CONSTRUCTOR})
public @interface Greeting {
    
}
//上述示例使用RetentionPolicy.RUNTIME，这样注解处理器可以通过反射，获取到该注解的属性值，从而去做一些运行时的逻辑处理。
```

### 五、 **@Documented**  文档化功能

 用于描述其它类型的annotation应该被作为被标注的程序成员的公共API，因此可以被例如javadoc此类的工具文档化。它是一个标记注解，没有成员。 

代码示例

```java
@Documented @Retention(RetentionPolicy.RUNTIME)
@Target({ElementType.METHOD,ElementType.CONSTRUCTOR})

public @interface Greeting {
}

```



### 六、@Inherited  标注继承

 用于表示某个被标注的类型是被继承的。如果一个使用了@Inherited修饰的annotation类型被用于一个class，则这个annotation将被用于该class的子类。 

代码示例

```java
@Inherited @Documented
@Retention(RetentionPolicy.RUNTIME)
@Target({ElementType.METHOD,ElementType.CONSTRUCTOR})
public @interface Greeting {
    
}
```

### 七、自定义注解实例

#### 7.1、自定义注解赋值

```java
@Target(value = ElementType.TYPE)//作用于类、接口、枚举
@Retention(RetentionPolicy.RUNTIME)//在运行时有效
public @interface PersonDefalutAnnotation {
    public String userName ();
    public String gender() default "男";
    public int age() default 1;
    //public Class clazz();
}
```



```java
@PersonDefalutAnnotation(userName = "张三",gender = "女",age = 20)
public class Person {
    private String userName;
    private String gender;
    private String age;

    public String getUserName() {
        return userName;
    }

    public String getGender() {
        return gender;
    }

    public String getAge() {
        return age;
    }
}
```

测试类

```java
/**测试人员数据封装注解的应用*/
public class TestPerson {
    public  Person getPersonInstance() throws Exception {
        //获得人员类的信息对象
        Class clazz = Person.class;
        //获得注解
        PersonDefalutAnnotation pda = (PersonDefalutAnnotation) clazz.getAnnotation(PersonDefalutAnnotation.class);
        //获得注解属性
        String userName = pda.userName();
        String gender = pda.gender();
        int age = pda.age();

        //创建此 Class 对象所表示的类的一个新实例
        Person person = (Person) clazz.newInstance();
        //返回一个 Field 对象，该对象反映此 Class 对象所表示的类或接口的指定已声明字段
        Field field = clazz.getDeclaredField("userName");
        //设置可访问权限
        field.setAccessible(true);
        field.set(person,userName);
        field = clazz.getDeclaredField("gender");
        field.setAccessible(true);
        field.set(person,gender);
        field = clazz.getDeclaredField("age");
        field.setAccessible(true);
        field.set(person,age);
        return person;
    }


    public  Person getPersonInstance2() throws Exception {
        Class clazz = Person.class;
        PersonDefalutAnnotation pda = (PersonDefalutAnnotation) clazz.getDeclaredAnnotation(PersonDefalutAnnotation.class);
        //创建人员对象
        Person person = (Person) clazz.newInstance();
        Field []fields = clazz.getDeclaredFields();
        for(Field field:fields){
            //取消访问权限检测
            field.setAccessible(true);
            //获得注解的Class对象
            Class aclazz = pda.getClass();
            //获取当前属性对象注解中的属性（反射中当成注解处理）
            Method method = aclazz.getMethod(field.getName());
            //访问方法，获取注解中的值
            Object value = method.invoke(pda);
            //给属性赋值
            field.set(person,value);
        }
        return person;


    }


    public static void main(String[] args) throws Exception {
        TestPerson testPerson = new TestPerson();
        Person person = testPerson.getPersonInstance2();
        System.out.println("userName:"+person.getUserName()+",gender:"+person.getGender()+",age:"+person.getAge());
    }

//userName:张三,gender:女,age:20
```

#### 7.2、自定义日期注解

```java
/**自定义日期注解*/
@Target(ElementType.FIELD)
@Retention(RetentionPolicy.RUNTIME)
public @interface DateAnnotation {
    String value() default "";
    String pattern() default "yyyy-MM-dd HH:mm:ss";
}
```

```java
/**职员类*/
public class Employee {
    private String name;//姓名
    @DateAnnotation()
    //@DateAnnotation(value="2016/12/13",pattern="yyyy/MM/dd")
    private Date hiredate;//入职时间

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public Date getHiredate() {
        return hiredate;
    }

    public void setHiredate(Date hiredate) {
        this.hiredate = hiredate;
    }

}
```

```java
/**测试注解*/
public class TestEmployee {
    //创建日期格式化对象
    private SimpleDateFormat dateFormat = new SimpleDateFormat();

    //使用反射获得对象，同时解析注解
    public Object getInstance(String className) throws Exception{
        Class clazz = Class.forName(className);
        Object object = clazz.newInstance();
        //获得所有属性
        Field[] fields = clazz.getDeclaredFields();
        for(Field field:fields){
            field.setAccessible(true);
            Annotation[] annotations = field.getDeclaredAnnotations();
            for (Annotation annotation:annotations){
                if(annotation instanceof DateAnnotation){
                    //进行向下转型
                    DateAnnotation dateAnnotation = (DateAnnotation) annotation;
                    //获取注解中的属性值
                    String value = dateAnnotation.value();
                    String pattern = dateAnnotation.pattern();
                    //判断value是否为空
                    if("".equals(value)){
                        //新建日期对象赋值注解标注的属性
                        field.set(object,new Date());
                    }else{
                        //修改日期格式
                        dateFormat.applyPattern(pattern);
                        //格式化时间value字符串,生成日期注解标注的属性
                        field.set(object,dateFormat.parse(value));
                    }
                }
            }
        }
        return object;

    }

    public static void main(String[] args) {
        try {
            TestEmployee testEmployee = new TestEmployee();
            SimpleDateFormat dateFormat =testEmployee.dateFormat;
            Employee employee = (Employee) testEmployee.getInstance("com.xzy.springcloud.com.demo.annotation.Employee");
            System.out.println(employee.getHiredate());
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```



















