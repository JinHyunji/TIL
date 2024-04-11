# Dynamic SQL

## 동적 SQL

- Runtime 시점에서 생성되는 SQL
- 사용자의 입력 혹은 특정 조건에 따라 동적으로 SQL을 생성하여 실행하는 방식
- MyBatis를 활용하면 동적 SQL을 보다 편리하게 사용할 수 있음
- JSTL이나 XML 기반의 텍스트 프로세서와 비슷한 느낌

<br>

## 동적 SQL 사용하는 이유

- 유연성 → 실행 중 SQL 쿼리를 조건에 따라 동적으로 생성할 수 있음. 다양한 상황에 따른 SQL 실행
- 조건부 → 사용자가 선택한 조건에 따라 WHERE 절 추가할 수 있음
- 정렬 → 동적으로 정렬 조건을 추가할 수 있음
- 보안 문제와 SQL Injection 공격에 노출될 수 있음 (주의 필요)

<br>

## MyBatis 동적 SQL 종류

- if
- choose (when, otherwise)
- trim (where, set)
- foreach

<br>

## 동적 SQL 예시

```html
...
<form action="search" method="GET" class="row lh-base">
	<div class="col-2">
		<label class="form-label">검색기준</label> 
		<select class="form-select" name="key">
			<option value="none" selected="selected">없음</option>
			<option value="writer">쓰니</option>
			<option value="title">제목</option>
			<option value="content">내용</option>
		</select>
	</div>
	<div class="col-5">
		<label class="form-label">검색 내용</label> 
		<input type="text" name="word" class="form-control">
	</div>
	<div class="col-2">
		<label class="form-label">정렬기준</label> 
		<select class="form-select" name="orderBy">
			<option value="none" selected="selected">없음</option>
			<option value="writer">쓰니</option>
			<option value="title">제목</option>
			<option value="view_cnt">조회수</option>
		</select>
	</div>
	<div class="col-2">
		<label class="form-label">정렬방향</label> 
		<select class="form-select" name="orderByDir">
			<option value="asc">오름차순</option>
			<option value="desc">내림차순</option>
		</select>
	</div>
	<div class="col-1 d-flex align-items-end">
		<input type="submit" value="검색" class="btn btn-primary">
	</div>
</form>
...
```

```xml
<!-- 검색 기능 -->
<select id="search" parameterType="SearchCondition"
	resultMap="boardMap">
	SELECT id, content, writer,
	title, view_cnt, date_format(reg_date,
	'%y-%m-%d') AS reg_date
	FROM board

	<!-- 동적 쿼리(검색 조건) -->
	<if test="key != 'none'">
		WHERE ${key} LIKE CONCAT('%', #{word}, '%')
	</if>

	<!-- 동적 쿼리(정렬 조건) -->
	<if test="orderBy != 'none'">
		ORDER BY ${orderBy} ${orderByDir}
	</if>
</select>
```

<br>
<br>
<br>



# Spring TX

## Transaction

- 데이터 베이스의 상태를 변화시키기 위해 수행하는 논리적인 작업의 단위
- 원자성 : 트랜잭션의 작업은 모두 수행되거나, 전혀 수행되지 않아야 한다.
- 일관성 : 트랜잭션은 데이터베이스를 일관성 있는 상태로 유지해야 한다.
- 격리성 : 동시에 실행되는 트랜잭션들이 서로 영향을 미치지 않아야 한다.
- 지속성 : 트랜잭션이 성공적으로 완료되면, 그 결과는 영구적으로 반영되어야 한다.

<br>

## Transaction 동작 방식

![Untitled](/img/트랜잭션%20동작방식.png)

<br>

## Spring TX

- Spring 에서 제공하는 트랜잭션을 기능을 활용할 수 있음
- @Transactional 을 활용하여 트랜잭션을 적용할 메서드를 선언한다.
- 해당 어노테이션이 있다면 자동으로 트랜잭션을 시작하고, 정상 종료 시 commit, 오류 발생 시 rollback 수행

<br>

## Spring TX 동작과정

- 트랜잭션 관리자를 통해 수행
- @Transactional 을 사용하면 Spring AOP를 통해 AOP 프록시 객체를 생성하고 이 프록시 객체가 관리자에게 처리를 위임한다.
- 따라서 사용자가 직접 관리할 필요 없이 선언만으로 트랜잭션을 관리할 수 있다.

![Untitled](/img/Spring%20TX%20동작과정.png)

<br>

## Spring TX 사용

- jar 또는 pom.xml 에 의존성 추가

```xml
<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-tx</artifactId>
			<version>${org.springframework-version}</version>
		</dependency>
```

- 트랜잭션 관리자 설정

```xml
<!-- 트랜잭션 매니저를 등록 -->
<bean id="transactionManager"
	class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
	<constructor-arg ref="dataSource"></constructor-arg>
</bean>
```

- 어노테이션 기반 트랜잭션 설정

```xml
<!-- 어노테이션 방식으로 트랜잭션을 관리해야겠다. -->
<tx:annotation-driven transaction-manager="transactionManager"/>
```

- 메서드나 클래스에 @Transactional 선언

```java
@Transactional
@Override
public void writeBoard(Board board) {
	boardDao.insertBoard(board);
}

@Transactional
@Override
public void removeBoard(int id) {
	boardDao.deleteBoard(id);
}

@Transactional
@Override
public void modifyBoard(Board board) {
	boardDao.updateBoard(board);
}
```