# Spring Framework

## Framework?

- 사전적 의미 : 뼈대, 틀
- SW에서의 의미 : SW 특정 문제를 해결하기 위해서 상호 협력하는 클래스와 인터페이스의 집합

<br>

## Framework 왜 사용하는가?

- 웹 어플리케이션을 개발하기 위해서는 많은 기본 기능을 설계, 작성해야 한다. (요청 처리, 세션 관리, 리소스 관리, 멀티 쓰레드 등)
- 공통으로 사용되는 기본 기능들을 일관되게 사용할 수 있으면 개발자는 웹 어플리케이션 기능 자체 개발에만 신경을 쓰면 되기 때문에 생산성이 높아진다.
- 개발자 입장에서는 완성된 구조에 맡은 기능을 개발하여 넣어주면 되기 때문에 개발 시간을 단축할 수 있다.

<br>

## Spring Framework의 등장

- 자바에서는 EJB(Enterprise JavaBeans)를 이용하여 엔터프라이즈 급 어플리케이션 제작
    - 경량 컨테이너 사용
    - 의존성 주입
    - AOP
    - POJO

<br>

## Spring Framework의 특징

- POJO (Plain Old Java Object) 방식의 프레임워크
- 의존성 주입 (Dependency Injection)을 통한 객체 관계 구성
- 관점 지향 프로그래밍 (AOP, Aspect Oriented Programming)
- 제어 역전 (IoC, Inversion of Control)
- 높은 확장성과 다양한 라이브러리
- …
 
<br>

![Untitled](/img/Spring%20Framework%20Runtime.png)

 
<br> 
<br>
<br>

# 의존 관계 역전


<br> 
<br>
<br>

# 의존성 주입

<br> 
<br>
<br>

# Spring Container Build
### Container?

- 스프링에서 핵심적인 역할을 하는 객체를 Bean이라고 하며,
- Container는 Bean의 인스턴스화 조립, 관리의 역할, 사용 소멸에 대한 처리를 담당한다.

![Untitled](/img/container.png)

- BeanFactory
    - 프레임워크 설정과 기본 기능을 제공하는 컨테이너
    - 모든 유형의 객체를 관리할 수 있는 메커니즘 제공
- ApplicationContext
    - BeanFactory 하위 인터페이스
    - 이벤트 처리, 국제화용 메시지 처리 등 다양한 기능 제공
- WebApplicationContext
    - 웹 환경에서 Spring을 사용하기 위한 기능이 추가됨
    - 대표적인 구현 클래스로 XmlWebApplicationContext가 있음

<br>

### 스프링 설정 정보 (Spring configuration metadata)

- 애플리케이션 작성을 위해 생성할 Bean과 설정 정보, 의존성 등의 방법을 나타내는 정보
- 설정 정보를 작성하는 방법은 XML 방식, Annotation 방식, Java Config 방식이 있다.
- 설정 정보 예)
    
    ![Untitled](/img/스프링%20설정%20정보.png)
    

<br>

### Spring Container 빌드

- Project 생성 후 → Maven Project로 변경
- pom.xml → Spring Context 의존성 추가

![Untitled](/img/스프링%20컨테이너%20빌드.png)

- Source Folder 생성 (resources)
- 스프링 설정 파일 생성 (XML 파일 → applicationContext.xml)
- 설정 공식 홈페이지에서 복사 가져오기

![Untitled](/img/복사.png)

- 빈 등록 (풀패키지 명 작성)

![Untitled](/img/빈%20등록.png)

- 스프링 컨테이너를 이용하여 객체 가져오기

![Untitled](/img/객체%20가져오기.png)

<br>

### 객체를 여러 개 가져와보기

![Untitled](/img/객체%20여러개.png)

<br>

### Bean Scope

- Bean 정의를 작성하는 것은 Bean 객체를 생성하는 것과는 다르다.
- Bean 범위(Scope)를 정의해서 객체의 범위를 제어할 수 있다.
- Scopes
![Untitled](/img/빈%20영역.png)

<br> 
<br>
<br>


# Spring DI - XML
### 의존성 주입 (생성자)

- constructor-arg 를 이용하여 의존성 주입
- <ref>, <value> 와 같이 하위 태그를 이용하여 설정 or 속성을 이용하여 설정

![Untitled](/img/의존성%20주입%20생성자.png)

<br>

### 의존성 주입 (설정자)

- setter 를 이용하여 의존성 주입
- <ref>, <value> 와 같이 하위 태그를 이용하여 설정 or 속성을 이용하여 설정
![Untitled](/img/의존성%20주입%20설정자.png)
- <property> 는 setter와 연결되어 위와 같은 경우 setComputer()를 이용해 값을 주입한다.
- 기본 자료형은 value에 넣고, 참조형은 ref에 넣는다.
- <property> 태그를 <bean> 태그의 `p:` 속정으로 바꿔서 사용할 수 있다.

<br> 
<br>
<br>

# Spring DI - Annotation
### 빈 (Bean) 생성 및 설정 (@Component)

- Bean 생성 - @Component
- 생성되는 bean의 이름은 클래스의 첫 글자를 소문자로 바꾼 것이다. 예) Desktop → desktop
    
    ![Untitled](/img/빈%20네임.png)
    
- 스프링은 @Component, @Service, @Controller, @Repository의 Stereotype Annotation을 제공
- 각 @Repository, @Service, @Controller 는 목적에 맞는 구체적인 사용을 위한 @Component의 확장. 목적에 맞게 구체화하여 사용하면 Spring 에서 더 효율적으로 사용 가능

<br>

### 빈 (Bean) 생성 및 설정 (Component Scan)

![Untitled](/img/빈%20생성%20및%20설정.png)

<br>

### 의존성 주입 (@Autowired)

![Untitled](/img/의존성%20주입.png)

- Spring 컨테이너 내에서 타입 매칭 시행 (컨테이너에 해당하는 타입의 bean이 있다면 매칭)

<br>

### @Autowired 사용 가능 위치

- 생성자
    
    ![Untitled](/img/오토%20생성자.png)
    
- Setter
    
    ![Untitled](/img/오토%20새터.png)
    
- field
    ![Untitled](/img/오토%20필드.png)

<br> 
<br>
<br>

# Spring DI - Java Config
### Java Config

![Untitled](/img/자바%20컨피그.png)