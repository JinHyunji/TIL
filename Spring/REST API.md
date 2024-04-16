# REST API

## REST

- 2000년도 로이 필딩의 박사 학위 논문에 최초로 소개
- 웹의 장점을 최대한 활용할 수 있는 아키텍처(설계 구조)로 REST 발표
- ‘Representational State Transfer’의 약어
- HTTP 프로토콜을 사용하여 데이터를 주고 받는 방법
- HTTP URI를 통해 제어할 자원을 명시하고, HTTP Method(GET, POST, PUT, DELETE)를 통해 해당 자원을 제어하는 명령을 내리는 방식의 아키텍처

<br>

## REST 구성

- 자원(Resource) → URI (Uniform Resource Identifier)
- HTTP 메서드 → 작업에 대한 정의 ( C / R / U / D )
- 표현(Representation) → Client와 Server 간 자원의 상태를 전달하는 방법으로  JSON, XML 등의 형식을 사용한다.

![Untitled](/img/rest%20구성.png)

<br>

## API

- Application Programming Interface
- 다른 소프트웨어 어플리케이션에서 사용할 수 있는 기능을 제공하는 인터페이스
- 어플리케이션 간 데이터를 교환하고 상호 작용하도록 도움을 줌
- 예 : 미세먼지 정보 제공 시스템, 핸드폰 정보 미세먼지 앱

![Untitled](/img/API.png)

<br>

## API 유형

- Private API
    - 비공개 API
    - 내부 시스템 또는 서비스 간의 통신을 위해 사용되는 API로 외부에 공개되지 않음
    - 주로 기업 내부 시스템에서의 통신을 위해 사용
    
- Public API
    - 공개 API
    - 외부 사용자 혹은 외부 어플리케이션과 상호작용하기 위해 공개된 API
    - 사용에 대한 권한 설정과 비용이 있을 수도 있음
    - 공공데이터 포털, 기상청, Naver, Kakao, Youtube 등 다양한 API가 존재함
    - 대부분이 REST 방식으로 작성되어 있음

<br>

## REST API (REST + API)

- 기존의 전송 방식과는 달리 서버는 요청으로 받은 리소스에 대해 순수한 데이터를 전송
- 기존의 GET/POST 외에 PUT, DELETE 방식을 사용하여 리소스에 대한 CRUD 처리 가능
- HTTP URI 를 통해 제어할 자원을 명시하고, HTTP method(GET/POST/PUT/DELETE)를 통해 해당 자원을 제어하는 명령을 내리는 방식의 아키텍처
- 표준이 정해진 것이 없어 관례 정도로 사용하는 Rule이 있음
    - 하이픈(-)은 사용 가능하지만 언더바(_)는 사용하지 않는다.
    - 특별한 경우를 제외하고 대문자 사용은 하지 않는다. (대소문자 구분하기 때문)
    - URI 마지막에 슬래시(/)를 사용하지 않는다.
    - 슬래시로 계층 관계를 나타낸다.
    - 확장자가 포함된 파일 이름을 직접 포함시키지 않는다.
    - URI는 명사를 사용한다.

<br>

## 기존 Service 와 REST API Service

- 기존 Service : 요청에 대한 처리를 한 후 가공된 data를 이용하여 특정 플랫폼에 적합한 형태의 View로 만들어서 반환
- REST API Service : data 처리만 하거나 처리 후 반환될 data가 있다면 JSON이나 XML 형식으로 전달. View에 대해서 신경 X → Open API에서 많이 사용

![Untitled](/img/api%20작동%20방식%20차이.png)

<br>

## API URI 설계 (기존)

- 기존의 접근 방식은 GET과 POST 만으로 자원에 대한 CRUD를 처리
- URI에 해당 기능을 추가 작성함

| 기능 | HTTP method | URI |
| --- | --- | --- |
| 게시글 목록 조회 | GET | /board/list |
| 게시글 상세 조회 | GET | /board/detail?id=2 |
| 게시글 생성 | POST | /board/write |
| 게시글 수정 | POST | /board/update |
| 게시글 삭제 | POST | /board/delete?id=2 |

<br>

## API URI 설계 (REST API)

- URI는 board라는 Resource를 활용하여 식별
- HTTP method를 통해 C / R / U/ D 행위를 구분

| 기능 | HTTP method | URI |
| --- | --- | --- |
| 게시글 목록 조회 | GET | /board |
| 게시글 상세 조회 | GET | /board/{id} |
| 게시글 생성 | POST | /board |
| 게시글 수정 | PUT / PATCH | /board |
| 게시글 삭제 | DELETE | /board/{id} |

<br>

## RESTful

- REST 아키텍처 스타일을 따르는 웹 서비스를 설계하고 구현하는 방식
- 플랫폼 독립성 → REST API는 특정 언어나 플랫폼에 종속되지 않음
- 높은 성능 → REST API는 cacheable 데이터를 지원하여 데이터를 캐시에 저장하고 빠르게 접근할 수 있음
- 간결함과 명확성 → REST API는 URI와 HTTP method를 이용하여 자원과 행위를 명확하게 표햔
- 표준화된 통신 → REST API는 HTTP를 통해 작동

![Untitled](/img/RESTful%20API.png)

<br>
<br>
<br>



# Spring REST API

## REST Client

- RESTful 웹 서비스에 HTTP 요청을 보내고 응답을 받는 프로그램 혹은 라이브러리
- HTTP 요청 전송 / 응답 수신 가능
- 대표적인 client로는 cURL, Postman, Talend API (크롬 웹스토어 설치) 등 …

![Untitled](/img/Jackson.png)

<br>

## Spring REST 관련 Annotation / Class

| Annotation / Class | Description |
| --- | --- |
| @ResponseBody | JSP 같은 뷰로 전달되는 것이 아니라 데이터 자체를 전달 |
| @RestController | Controller가 REST 방식을 처리하기 위한 것임을 명시 |
| @GetMapping <br> @PostMapping <br> @PutMapping <br> @DeleteMapping | 요청 방식 |
| @PathVariable | URL 경로에 있는 값을 파라미터로 추출 |
| @RequestBody | JSON 데이터를 원하는 타입으로 바인딩 |
| ResponseEntity | 데이터 응답시 [상태코드, 헤더, 데이터] 설정이 가능 |
| @CrossOrigin | Ajax의 크로스 도메인 문제를 해결 |

<br>

## @ResponseBody

- controller 메서드가 HTTP 응답의 본문(Body)을 직접 반환함을 나타내는 Annotation
- 기존의 @Controller는 뷰 리졸버를 통해 View를 찾지만 데이터를 반환하기 위해서 해당 Annotation을 사용해야 함
- Spring은 반환된 객체를 JSON, XML 등의 형식으로 변환하여 HTTP 응답 본문으로 Client에게 전송

<br>

## @Jackson Databind

- Jackson 라이브러리의 일부
- Java 객체와 JSON 데이터 간의 변환을 담당
- 별도의 Annotation 없이 자동으로 Java 객체와 JSON 데이터를 매핑할 수 있음
- JSON 외에도 다양한 데이터 변환 지원
- jar 파일 혹은 pom.xml을 통해 의존성을 추가하여 사용

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/6f495fce-27d3-4db6-901b-54033b9f2570/8d3173c1-0691-475d-a572-e0983afd7ff8/Untitled.png)

<br>

## @RestController

- spring MVC에서 RESTful 서비스를 개발할 때 주로 사용함
- 해당 Annotation을 활용하면 모든 메서드가 @ResponseBody를 포함하게 됨
- 따라서 반환객체가 HTTP 응답 본문에 작성되며, JSON 또는 XML 등의 형태로 전송 가능
- @Controller + @ResponseBody

<br>

## HTTP method에 따른 Annotation

- @GetMapping → 데이터를 조회하는데 사용
- @PostMapping → 데이터를 생성하는데 사용
- @PutMapping → 데이터를 수정하는데 사용
- @DeleteMapping → 데이터를 삭제하는데 사용

<br>

## @PathVariable

- URI의 일부를 변수로 가져와 메서드의 매개변수로 전달할 때 사용
- RESTful 웹 서비스에서 경로 변수를 처리하는데 사용

<br>

## @RequestBody

- HTTP 요청의 본문에 포함되어 있는 데이터를 Java 객체로 변환할 때 사용
- RESTful 웹 서비스에서 Client가 전송한 데이터를 서버에서 받아서 처리하는데 사용
- form-data로 요청 전송 시 @ModelAttribute를 사용하여 처리
- JSON 형태의 요청 전송 시 @RequestBody를 사용하여 처리

<br>

## ResponseEntity

- HTTP 응답을 나타내는 클래스
- 상태 코드 지정 → HTTP 응답의 상태 코드를 지정할 수 있음
- 헤더 추가 → HTTP 응답에 추가적인 헤더를 포함할 수 있음
- 본문 설정 → HTTP 응답의 본문을 설정할 수 있음