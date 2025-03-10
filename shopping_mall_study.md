## JPA (Java Persistence API) 
#### 정의: 자바 ORM(Object Relational Mapping) 기술에 대한 표준 API



<br/>

## ORM (Object Relational Mapping)
#### 정의: ORM은 객체와 관계형 데이터베이스를 매핑해주는 기술

#### JPA 사용 시 장점
1. ##### 데이터베이스 독립성: 특정 데이터베이스에 종속되지 않음.
2. ##### 객체지향적 프로그래밍
   - ###### 데이터베이스 설계 중심의 패러다임에서 객체지향적 패러다임으로 전환.
   - ###### 직관적이며 비즈니스 로직에 집중할 수 있도록 도와줌.
3. ##### 생산성 향상
   - ###### SQL문을 직접 작성하지 않고 객체를 사용하여 동작.
   - ###### 유지보수 측면에서 유리하며 재사용성이 증가함.
   
#### JPA 사용 시 단점
1. ##### 복잡한 쿼리 처리
   - ###### 통계 처리와 같은 복잡한 쿼리를 사용할 경우 SQL문을 사용하는 것이 나을 수 있음.
2. ##### 성능저하 위험 
   - ###### 객체 간 매핑 설계를 잘못했을 때 성능 저하가 발생할 수 있음.
   - ###### 자동으로 생성되는 쿼리가 많아 개발자가 의도하지 않는 쿼리로 인해 성능이 저하될 수 있음.
3. ##### 학습시간
   - ###### JPA를 배우는 데 시간이 소요될 수 있음.



<br>

## 엔티티 Entity
#### 정의: 데이터 베이스의 테이블에 대응하는 클래스다. @Entity가 붙은 클래스는 JPA 에서 관리하며 엔티티라고 한다. 
#### 클래스 자체나 생성된 인스턴스도 엔티티라고 부른다. 

<br/>

### 엔티티 매니저 팩토리 Entity Manager Factory
#### 엔티티 매니저 인스턴스를 관리하는 주체다. 애플리케이션 실행 시 한 개만 만들어지고, 사용자로부터 요청이 오면 엔티티 매니저 팩토리로부터 엔티티 매니저를 생성한다.

<br/>

### 엔티티 매니저 Entity Manager
#### 영속성 컨텍스트에 접근하여 엔티티에 대한 데이터베이스 작업을 제공한다. 내부적으로 데이터베이스 커넥션을 사용해서 데이터베이스에 접근한다.

<br/>

### 영속성 컨텍스트 Persistence Context
#### 엔티티를 영구 저장하는 환경으로 엔티티 매니저를 통해 영속성 컨텍스트에 접근한다.



<br>

---- 

<br>

#### 쿼리 메소드
#### 는 간단한 쿼리를 처리할 때는 유용하지만 복잡한 쿼리를 다루기에는 적합하지 않다.
#### 이를 보완하기 위해선? Spring Data JPA 에서 제공하는 
### `@Query 어노테이션`
#### 을 이용하면 SQL 과 유사한 JPQL 이라는 객체지향 쿼리 언어를 통해 통잡한 쿼리도 처리가 가능하다.
#### 하지만 @Query 어노테이션을 이용한 방법에도 단점이 있다. JPQL 문법으로 문자열을 입력하기 때문에 잘못 입력하면 컴파일 시점에 에러를 발견할 수 없다.
#### 이를 보완할 수 있는 방법? -> `Querydsl`

<br>

- #### Querydsl 의 장점?
  1. ##### 고정된 SQL문이 아닌 조건에 맞게 동적응로 쿼리를 생성할 수 있음
  2. ##### 비슷한 쿼리를 재사용할 수 있다
  3. ##### 문자열이 아닌 자바 소스코드로 작성하기 때문에 컴파일 시점에 오류를 발견할 수 있다
  4. ##### IDE 자동완성 기능을 이용할 수 있기 때문에 생산성 향상!

<br>

- #### 사용방법?
  1. ##### pom.xml 의존성 추가하기
  2. ##### Qdomain 자바 코드를 생성하는 플러그인 추가하기 (엔티티를 기반으로 접두사로 Q가 붙는 클래스들을 자동으로 생성해주는 플러그인이다.)
   


<br>

## Thymeleaf
#### Thymeleaf는 화면을 동적으로 생성하기 위한 템플릿 엔진으로, 미리 정의된 템플릿을 사용하여 요청 시마다 새로운 HTML 페이지를 서버에서 생성하여 클라이언트에 전달하는 서버 사이드 렌더링 방식이다.

#### `서버 사이드 렌더링 방식` 이라고 말한다. 
- #### 서버 사이드 템플릿 엔진
  1. ##### Thymeleaf
  2. ##### JSP
  3. ##### Freemarker
  4. ##### Groovy
  5. ##### Mustache 등 
- #### Thymeleaf 장점
  ##### Thymeleaf의 가장 큰 장점은 'natural templates'이다. JSP 파일은 .JSP 확장자를 가지며, 서버 사이드 렌더링을 하지 않으면 JSP 문법이 화면에 나타난다. 반면, Thymeleaf는 .html 확장자를 가지며, Thymeleaf 문법을 포함한 HTML 파일을 브라우저에서 직접 열어도 정상적인 화면을 볼 수 있다. Thymeleaf 문법은 HTML 태그의 속성으로 사용된다.


<br>

## Spring Boot Devtools
#### 애플리케이션 개발 시 유용한 기능들을 제공하는 모듈이다. 개발 생산성을 향상시키는 데 도움이 된다.

- #### Spring Boot Devtool 에서 제공하는 대표적인 기능
  1. ##### Automatic Restart : classpath에 있는 파일이 변경될 때마다 애플리케이션을 자동으로 재시작 해줌
  2. ##### Live Reload : 정적 자원 (html,css,js) 수정 시 새로고침 없이 바로 적용 가능
  3. ##### Property Defaults : Thymeleaf는 기본적으로 성능을 향상시키기 위해서 캐싱 기능을 사용한다. 하지만 개발하는 과정에서 캐싱 기능을 사용한다면 수정한 소스가 제대로 반영되지 않을 수 있기 때문에 cache 의 기본값을 false로 설정할 수 있다
- #### 위 기능들을 추가하려면 pom.xm에 spring-boot-devtools 의존성 추가 후 "Reload All Maven Projects"을 클릭하여 의존성을 받아와야 한다.


<br>

## th:each
#### 자바의 for문처럼 '반복문'을 사용할 수 있다. Thymeleaf에서는 th:each 문법을 사용한다

`<tr th:each="itemDto, status: ${itemDtoList}">`

전달받은 itemDtoList에 있는 데이터를 하나씩 꺼내와서 
itemDto에 담아주고 
status에는 현재 반복에 대한 상태 데이터가 존재한다. 
변수명은 status 대신 다른 걸 사용해도 됨.

````
<tr th:each="itemDto, status: ${itemDtoList}">

    <td th:text="${status.index}"></td> 
    //현재 순회하고 있는 데이터의 인덱스 출력

    <td th:text="${itemDto.itemNm}"></td>
    <td th:text="${itemDto.itemDetail}"></td>
    <td th:text="${itemDto.price}"></td>
    <td th:text="${itemDto.regTime}"></td>
</tr>
````


<br>

## th:if, th:unless
#### 자바에서 if-else와 같은, '조건문'을 사용할 수 있다. 아래 코드에 조건을 줘서 index 번호가 홀수 짝수로 출력됨.. Thymeleaf에서는 th:if, th:unless 문법을 사용한다

````
<tr th:each="itemDto, status : ${itemDtoList}">

    <td th:if="${status.even}" th:text="짝수"></td>
    <td th:unless="${status.even}" th:text="홀수"></td>
<!--  <td th:text="${status.index}"></td>-->

    <td th:text="${itemDto.itemNm}"></td>
    <td th:text="${itemDto.itemDetail}"></td>
    <td th:text="${itemDto.price}"></td>
    <td th:text="${itemDto.regTime}"></td>
</tr>
````


<br>

## th:switch, th:case
#### 자바에서 switch-case문과 같은, 여러개의 조건을 처리해야 할 때 사용한다. 위 코드와 실행했을때 동일한 결과가 출력된다. Thymeleaf에서는 th:switch, th:case 문법을 사용한다

````
      <td th:switch="${status.even}">
        <span th:case="true">짝수</span>
        <span th:case="false">홀수</span>
      </td>
````


<br>

## th:href
####  Thymeleaf에서는 링크를 처리하는 문법

```
th:href=@{이동할 경로}
```


<br>

## Thymeleaf 페이지 레이아웃
#### 공통요소 관리를 쉽게 하기 위해 사용한다
#### thymeleaf-layout-dialect 라이브러리 설치 

```
<dependency>
    <groupId>nz.net.ultraq.thymeleaf</groupId>
    <artifactId>thymeleaf-layout-dialect</artifactId>
    <version>3.3.0</version>
</dependency>
```

##### 하나의 레이아웃을 미리 만들어놓고 현재 작성중인 페이지만 레이아웃에 끼워넣으면 된다.



<br/>

## security
#### 스프링 시큐리티는 스프링 기반의 애플리케이션을 위한 보안 솔루션을 제공한다. 애플리케이션을 만들기 위해선 보통 인증/인가 등의 보안이 필요하다.
#### security dependency 추가하기

```
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-security</artifactId>
</dependency>
```

##### pom.xml에 security 와 관련된 의존성 추가하고 받아오면 이제 모든 요청은 인증을 필요로 한다. 
##### 기본적으로 제공하는 아이디는 user 고 비밀번호는 애플리케이션을 실행할 때마다 콘솔창에 출력되는 것을 볼 수 있다 

```
Using generated security password: 85bbf8e4-5711-45b4-b56d-99068283d3cc
```

#### 로그아웃 기능도 제공한다. URL에 localhost/logout 을 입력하면 된다. 하지만 현재는 user 계정 밖에 없으며 비밀번호도 계속 바뀐다!.
