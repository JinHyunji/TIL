# Model 객체와 @ModelAttribute 어노테이션

## 생성자 주입 시 오류

- @ModelAttribute 애노테이션 사용 시 dto 객체에 기본생성자와 다른 생성자가 있다면 기본생성자로 객체를 생성한 다음 setter를 이용해서 데이터를 주입함.

<br>

## Model 객체

### 정의

- Model 객체는 Controller에서 생성된 데이터를 담아 View로 전달할 때 사용하는 객체이다.
- Servlet의 request.setAttribute()와 비슷한 역할을 한다.
- addAttribute(”key”, “value”) 메서드를 이용해 view에 전달할 데이터를 key, value 형식으로 전달할 수 있다.

### 사용 예시

```java
@Controller
public class TestController {
	@GetMapping("/test")
	public void testMethod(Model model) {
		String msg = "model test";
		model.addAttribute("key", msg);
	}
}
```

```html
<h2> Test Method : ${key} </h2>
```

<br>

## @ModelAttribute

### 정의

- @ModelAttribute는 HTTP Body 내용과 HTTP 파라미터의 값들을 Getter, Setter, 생성자를 통해 주입하기 위해 사용한다.
- 일반 변수의 경우 전달이 불가능하기 때문에 model 객체를 통해서 전달해야 한다.
- @ModelAttribute(”파라미터명”)

### 메커니즘

- Spring MVC에서 @ModelAttribute를 메소드의 파라미터로 사용할 경우 내부적으로 어떻게 돌아가는가

```java
@Controller
public class TestController {
	@GetMapping("/test")
	public void testMethod(Model model) {
		String msg = "model test";
		model.addAttribute("key", msg);
	}
	
	@GetMapping("/test2")
	public String test(@ModelAttribute("test") Test test, Model model) {
		model.addAttribute("result", service.getResult(test));
		return "test";
	}
}
```

- 뷰에서 name의 값이 Test 클래스의 인스턴스 명과 일치해야 자동으로 바인딩 된다.

```html
<tr>
	<th>TEST ID</th>
	<td><input type="text" name="testId" th:value="${test.testId}"></td>
</tr>
<tr>
	<th>TEST NAME</th>
	<td><input type="text" name="testName" th:value="${test.testName}"><td>
</tr>
<tr>
	<th>TEST PWD</th>
	<td><input type="text" name="testPwd" th:value="${test.testPwd}"/><td>
<tr>
<tr>
	<th><input type="submit" value="전송"/></th>
<tr>
```

- @ModelAttribute 선언 후 자동으로 진행되는 작업 순서
1. 파라미터로 넘겨 준 타입의 오브젝트를 자동으로 생성한다.
    1. 위의 코드로는 Test 클래스의 객체 test를 자동으로 생성한다. 이때 @ModelAttribute가 지정되는 클래스는 getter, setter,가 있는 Beans 클래스여야 한다.
2. 생성된 오브젝트(test) HTTP로 넘어 온 값들을 자동으로 바인딩한다.
    1. http://localhost:3001/biz/test?testId=1&testName=1&testPwd=1
    2. 위처럼 들어오는 값이 Test 클래스의 testId, testName, testPwd 각각 해당 변수의 setter를 통해서 해당 멤버 변수로 바인딩된다.
3. @ModelAttribute 가 붙은 객체가(Test 객체) 자동으로 Model 객체에 추가되고 View 단으로 전달된다.
    1. @ModelAttribute(”test”) Test test 에서 어노테이션 괄호 안의 test 값을 통해 view 단에서 데이터들을 호출할 수 있다.