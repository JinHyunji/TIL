# MyBatis-Spring

## MyBatis-Spring

- 마이바티스-스프링 연동 모듈은 둘을 간편하게 연동하도록 도와준다.
- 해당 모듈은 마이바티스로 하여금 스프링 트랜잭션에 쉽게 연동되도록 처리한다.
- mapper와 SqlSession을 다루고, 빈에 주입시켜준다.
- MyBatis 예외를 스프링의 DataAccessException 으로 반환한다.

<br>

## MyBatis-Spring 구성 요소


- SqlSessionFactoryBean → MyBatis와  Spring을 연결하는데 중요한 역할을 하는 Bean
- SqlSessionTemplate → Mybatis-Spring의 핵심 구현체로, SqlSession을 구현하고 코드에서 SqlSession을 대체하는 역할을 한다.

<br>

## MyBatis-Spring 시작하기

- mybatis-spring-x.x.x.jar 파일을 프로젝트에 추가
- maven 프로젝트를 사용한다면 pom.xml에 의존성 추가
```xml
<dependency>
    <groupId>org.mybatis</groupId>
    <artifactId>mybatis-spring</artifactId>
    <version>3.0.3</version>
</dependency> 
```

- MyBatis 실행에 필요한 Bean 들 설정 파일(root-context.xml) 등록하기
- DataSource, SqlSessionFactoryBean 등 …

```xml
<!-- DataSource가 필요해 -->
	<bean id="dataSource"
		class="org.springframework.jdbc.datasource.DriverManagerDataSource">
		<property name="driverClassName" value="com.mysql.cj.jdbc.Driver" />
		<property name="url"
			value="jdbc:mysql://localhost:3306/ssafy_board?serverTimezone=UTC" />
		<property name="username" value="ssafy" />
		<property name="password" value="ssafy" />
	</bean>

	<!-- 공장 세울 준비를 하자 -->
	<bean id="sqlSessionFactory"
		class="org.mybatis.spring.SqlSessionFactoryBean">
		<property name="dataSource" ref="dataSource" />
		<property name="mapperLocations"
			value="classpath:mappers/*.xml" />
		<property name="typeAliasesPackage"
			value="com.ssafy.board.model.dto" />
	</bean>
```

- Dao를 직접 등록하는 방법 (반드시 클래스가 아닌 인터페이스)

```xml
<bean id="boardDao" class="org.mybatis.spring.mapper.MapperFactoryBean">
    <property name="mapperInterface" value="dao.BoardDao"/>
    <property name="sqlSessionFactory" ref="sqlSessionFactory"/>
</bean>
```

- Mapper 스캔을 이용하여 Dao Interface 위치 지정

```xml
<mybatis:scan base-package="com.ssafy.board.model.dao"/>
```
