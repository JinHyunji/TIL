<br>

# 1. Root and child contexts
  - 모든 child context들은 root context에 정의된 bean에 접근할 수 있지만, root context는 child context의 bean에 접근 불가하다.

<br>

  ### 1.1. RootApplicationContext
- 최상위 ApplicationContext이다. 
- WebApplicationContext의 부모 Context이며 자식에게 자신의 설정을 공유한다. 
  - 단, 자신은 자식인 WebApplicationContext의 설정에 접근하지 못한다.

<br>

### 1.2. WebApplicationContext
- Servlet 단위의 ApplicationContext이다.
- RootApplicationContext의 자식 Context이며, 부모인 RootApplicationContext의 설정에 접근할 수 있다.

<br>
<br>
<br>


# 2. DispatcherServlet - Child application contexts
- DispatcherServlet은 Front controller라고도 하며 웹 요청을 받아 처리한다.
- web.xml 파일에 아래와 같이 작성한다.
```xml
<servlet>
	<servlet-name>employee-services</servlet-name>
	<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
	<init-param>
		<param-name>contextConfigLocation</param-name>
		<param-value>classpath:employee-services-servlet.xml</param-value>
	</init-param>
	<load-on-startup>1</load-on-startup>
</servlet>
```

<br>
<br>
<br>


# 3. ContextLoaderListener - Root application context
- ContextLoaderListener는 root application을 만든다. 이것은 child context에 공유된다.
- web.xml 파일에 한 번만 작성한다.
```xml
<listener>
  <listener-class>
    org.springframework.web.context.ContextLoaderListener
  </listener-class>
</listener>
 
<context-param>
  <param-name>contextConfigLocation</param-name>
  <param-value>/WEB-INF/spring/applicationContext.xml</param-value>
</context-param>
```

<br>
<br>
<br>


# 4. ContextLoaderListener vs DispatcherServlet
![img](https://howtodoinjava.com/wp-content/uploads/2018/05/ContextLoaderListener-vs-DispatcherServlet.png)


<br>
<br>
<br>


# Reference
- https://howtodoinjava.com/spring-mvc/contextloaderlistener-vs-dispatcherservlet/
- https://tlatmsrud.tistory.com/43