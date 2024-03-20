
<br>

# 1. 프로젝트 생성
- sts에서 프로젝트 진행
1. Dynamic Web Project 생성
2. Maven Project로 변환

<br>
<br>
<br>

# 2. Dependency 등록
1. [mvn repository](https://mvnrepository.com/) 에서 pom.xml에 아래 3개 dependency 등록
   1. [Spring Web MVC](https://mvnrepository.com/artifact/org.springframework/spring-webmvc)
   2. [Jakarta Standard Tag Library API](https://mvnrepository.com/artifact/jakarta.servlet.jsp.jstl/jakarta.servlet.jsp.jstl-api)
   3. [Jakarta Standard Tag Library Implementation](https://mvnrepository.com/artifact/org.glassfish.web/jakarta.servlet.jsp.jstl)
2. `<properties>` 에 버전 추가 후 dependency에 EL 태그로 버전 값 변경
```xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<groupId>BoardTest</groupId>
	<artifactId>BoardTest</artifactId>
	<version>0.0.1-SNAPSHOT</version>
	<packaging>war</packaging>
	<properties>
		<org.springframework-version>6.1.3</org.springframework-version>
		<jstl-version>3.0.0</jstl-version>
	</properties>
	<build>
		<plugins>
			<plugin>
				<artifactId>maven-compiler-plugin</artifactId>
				<version>3.8.1</version>
				<configuration>
					<release>17</release>
				</configuration>
			</plugin>
			<plugin>
				<artifactId>maven-war-plugin</artifactId>
				<version>3.2.3</version>
			</plugin>
		</plugins>
	</build>

	<dependencies>
		<!--
		https://mvnrepository.com/artifact/org.springframework/spring-webmvc -->
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-webmvc</artifactId>
			<version>${org.springframework-version}</version>
		</dependency>
		<!--
		https://mvnrepository.com/artifact/jakarta.servlet.jsp.jstl/jakarta.servlet.jsp.jstl-api -->
		<dependency>
			<groupId>jakarta.servlet.jsp.jstl</groupId>
			<artifactId>jakarta.servlet.jsp.jstl-api</artifactId>
			<version>${jstl-version}</version>
		</dependency>
		<!--
		https://mvnrepository.com/artifact/org.glassfish.web/jakarta.servlet.jsp.jstl -->
		<dependency>
			<groupId>org.glassfish.web</groupId>
			<artifactId>jakarta.servlet.jsp.jstl</artifactId>
			<version>${jstl-version}</version>
		</dependency>


	</dependencies>
</project>
```

<br>
<br>
<br>


# 3. servlet-context.xml
- 파일 위치 : /src/main/webapp/WEB-INF/spring/boardServlet/servlet-context.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:context="http://www.springframework.org/schema/context"
	xsi:schemaLocation="
		http://www.springframework.org/schema/beans https://www.springframework.org/schema/beans/spring-beans.xsd
		http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">

	<!-- 1. 핸들러 매핑과 뷰 리졸버 설정 -->
	<!-- 핸들러는 자동으로, 뷰 리졸버만 알아서 빈 등록 -->	
	<bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
		<property name="prefix" value="/WEB-INF/views"></property>
		<property name="suffix" value=".jsp"></property>
	</bean>
	
	<!-- bean definitions here -->
	<context:component-scan base-package="com.ssafy.board.controller"></context:component-scan>
</beans>
```

<br>
<br>
<br>


# 4. root-context.xml
- 파일 위치 : /src/main/webapp/WEB-INF/spring/root-context.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:context="http://www.springframework.org/schema/context"
	xsi:schemaLocation="
		http://www.springframework.org/schema/beans https://www.springframework.org/schema/beans/spring-beans.xsd
		http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">

	<context:component-scan base-package="com.ssafy.board.model"></context:component-scan>
</beans>
```