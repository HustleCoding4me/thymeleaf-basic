# thymeleaf-basic


#타임리프 기본 사용법
---

# 기본 문법 

• 간단한 표현:
    ◦ 변수 표현식: ${...}
    ◦ 선택 변수 표현식: *{...}
    ◦ 메시지 표현식: #{...}
    ◦ 링크 URL 표현식: @{...}
    ◦ 조각 표현식: ~{...}
• 리터럴
    ◦ 텍스트: 'one text', 'Another one!',…
    ◦ 숫자: 0, 34, 3.0, 12.3,…
    ◦ 불린: true, false
    ◦ 널: null
    ◦ 리터럴 토큰: one, sometext, main,…
• 문자 연산:
    ◦ 문자 합치기: +
    ◦ 리터럴 대체: |The name is ${name}|
• 산술 연산:
    ◦ Binary operators: +, -, *, /, %
    ◦ Minus sign (unary operator): -
• 불린 연산:
    ◦ Binary operators: and, or
    ◦ Boolean negation (unary operator): !, not
• 비교와 동등:
    ◦ 비교: >, <, >=, <= (gt, lt, ge, le)
    ◦ 동등 연산: ==, != (eq, ne)
• 조건 연산:
    ◦ If-then: (if) ? (then)
    ◦ If-then-else: (if) ? (then) : (else)
    ◦ Default: (value) ?: (defaultvalue)
• 특별한 토큰:
    ◦ No-Operation: _

https://www.thymeleaf.org/doc/tutorials/3.0/usingthymeleaf.html#standard-expression-syntax


---
## text, utext


* text : Controller에서 받은 문자 그대로 출력 (자동 Escape)

> escape (문자 그대로 변환시키기 위해 html Tag 등에 들어가는
> 특수 문자들을 다르게 치환하는 것)
>> Hello <b> Spring! </b> -> 
>> ex)<span>Hello &lt;b&gt; Spring ! &lt;/b&gt;</span>

```html
Text Escape
<li>th:text사용<span th:text="${data}"></span></li>
<li>컨텐츠 안에서 직접 출력하기 = [[${data}]]</li>
```

* utext : Unescape하여 태그가 있으면 htmlEntity로 변환하여 출력

```html
 UText Unescape
<li>th:utext사용<span th:utext="${data}"></span></li>
<li>컨텐츠 안에서 직접 출력하기 = [(${data})]</li>
```

---


### 변수 - SpringEL

* Object, List, Map 표현 방법

```html
<ul>Object
    <li>${user.username} = <span th:text="${user.username}"></span></li>
    <li>${user['username']} = <span th:text="${user['username']}"></span></li>
    <li>${user.getUsername()} = <span th:text="${user.getUsername()}"></span></li>
</ul>
<ul>List
    <li>${users[0].username} = <span th:text="${users[0].username}"></span></li>
    <li>${users[0]['username']} = <span th:text="${users[0]['username']}"></span></li>
    <li>${users[0].getUsername()} = <span th:text="${users[0].getUsername()}"></span></li>
</ul>
<ul>Map
    <li>${userMap['userA'].username} = <span th:text="${userMap['userA'].username}"></span></li>
    <li>${userMap['userA']['username']} = <span th:text="${userMap['userA']['username']}"></span></li>
    <li>${userMap['userA'].getUsername()} = <span th:text="${userMap['userA'].getUsername()}"></span></li>
</ul>

```

### 지역변수 with

* 지역 변수 선언
* 선언된 태그 안에서만 사용 가능하다.


```html
<h1>지역 변수 - (th:with)</h1>
<div th:with="first=${user[0]}">
    <p>처음 사람의 이름은 <span th:text="${first.username}"></span></p>
</div>
-- first란 지역변수에 user[0]을 삽입하여 사용.
```

------------------------------------------------


### 기본객체

Thymeleaf는 기본적으로 request, response, session, servletContext, local을 제공한다.

```html
<ul>
    <li>request = <span th:text="${#request}"></span></li>
    <li>response = <span th:text="${#response}"></span></li>
    <li>session = <span th:text="${#session}"></span></li>
    <li>servletContext = <span th:text="${#servletContext}"></span></li>
    <li>locale = <span th:text="${#locale}"></span></li>
</ul>
```


### 기본 편의 utils

> 타임리프 유틸리티 객체들
* message : 메시지, 국제화 처리
* uris : URI 이스케이프 지원
* dates : java.util.Date 서식 지원
* calendars : java.util.Calendar 서식 지원
* temporals : 자바8 날짜 서식 지원
* numbers : 숫자 서식 지원
* strings : 문자 관련 편의 기능
* objects : 객체 관련 기능 제공
* bools : boolean 관련 기능 제공
* arrays : 배열 관련 기능 제공
* lists , sets , maps : 컬렉션 관련 기능 제공
* ids : 아이디 처리 관련 기능 제공, 뒤에서 설명


기본 예시 : https://www.thymeleaf.org/doc/tutorials/3.0/usingthymeleaf.html#appendix-b-expression-utility-objects

### 필요한 java 8의 LocalDate, LocalDateTime, instant





