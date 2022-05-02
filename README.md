# thymeleaf-basic


#타임리프 기본 사용법
---

# 기본 문법 

• 간단한 표현:<br>
   ㅤ ◦ 변수 표현식: ${...}<br>
 ㅤ   ◦ 선택 변수 표현식: *{...}<br>
ㅤ    ◦ 메시지 표현식: #{...}<br>
ㅤ    ◦ 링크 URL 표현식: @{...}<br>
ㅤ    ◦ 조각 표현식: ~{...}<br>
• 리터럴<br>
  ㅤ  ◦ 텍스트: 'one text', 'Another one!',…<br>
 ㅤ   ◦ 숫자: 0, 34, 3.0, 12.3,…<br>
 ㅤ   ◦ 불린: true, false<br>
 ㅤ   ◦ 널: null<br>
 ㅤ   ◦ 리터럴 토큰: one, sometext, main,…<br>
• 문자 연산:<br>
   ㅤ ◦ 문자 합치기: +<br>
  ㅤ  ◦ 리터럴 대체: |The name is ${name}|<br>
• 산술 연산:<br>
  ㅤ  ◦ Binary operators: +, -, *, /, %<br>
 ㅤ   ◦ Minus sign (unary operator): -<br>
• 불린 연산:<br>
 ㅤ   ◦ Binary operators: and, or<br>
  ㅤ  ◦ Boolean negation (unary operator): !, not<br>
• 비교와 동등:<br>
   ㅤ ◦ 비교: >, <, >=, <= (gt, lt, ge, le)<br>
  ㅤ  ◦ 동등 연산: ==, != (eq, ne)<br>
• 조건 연산:<br>
 ㅤ   ◦ If-then: (if) ? (then)<br>
 ㅤ   ◦ If-then-else: (if) ? (then) : (else)<br>
 ㅤ   ◦ Default: (value) ?: (defaultvalue)<br>
• 특별한 토큰:<br>
  ㅤ  ◦ No-Operation: _<br>

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

> Thymeleaf는 기본적으로 request, response, session, servletContext, local을 제공한다.

```html
<ul>
    <li>request = <span th:text="${#request}"></span></li>
    <li>response = <span th:text="${#response}"></span></li>
    <li>session = <span th:text="${#session}"></span></li>
    <li>servletContext = <span th:text="${#servletContext}"></span></li>
    <li>locale = <span th:text="${#locale}"></span></li>
</ul>
```

> 기본 객체가 잡힌 모습

![image](https://user-images.githubusercontent.com/37995817/165952527-bba94a06-af26-45d3-b5a2-c0625308d55b.png)


---

* 기본 주어진 객체에서 data를 꺼내서 쓸 수도 있겠지만, 이도 편리하게 이미 구현되어있다.

```html
<ul>
    <li>Request Parameter = <span th:text="${param.paramData}"></span></li>
    <li>session = <span th:text="${session.sessionData}"></span></li>
    <li>spring bean = <span th:text="${@helloBean.hello('Spring!')}"></span></li>
</ul>
```


> 편의객체들

![image](https://user-images.githubusercontent.com/37995817/165952827-6a3b506d-2fe4-45ad-aefd-15769eb1fd17.png)

* request에 Parameter를 기본적으로 꺼내 쓸 수 있는 param

ex ) http://localhost:8090/basic/basic-objects?paramData=HelloParam 으로 paramData에 HelloParam을 넣은 모습

* session에 담긴 key값으로 데이터 꺼내기

```java
 @GetMapping("/basic-objects")
    public String basicObjects(HttpSession session) {
        session.setAttribute("sessionData", "Hello Session");
        return "basic/basic-objects";
    }
```

* 심지어 bean도 호출하여 method를 그릴 수도 있다.

```java
  @Component("helloBean")
    static class HelloBean {
        public String hello(String data) {
            return "Hello " + data;
        }
    }
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

### ★필요한 java 8의 LocalDate, LocalDateTime, instant

```html

<ul>
    <li>default = <span th:text="${localDateTime}"></span></li>
    <li>yyyy-MM-dd HH:mm:ss = <span th:text="${#temporals.format(localDateTime, 'yyyy-MM-dd HH:mm:ss')}"></span></li>
</ul>
<h1>LocalDateTime - Utils</h1>
<ul>
    <li>${#temporals.day(localDateTime)} = <span th:text="${#temporals.day(localDateTime)}"></span></li>
    <li>${#temporals.month(localDateTime)} = <span th:text="${#temporals.month(localDateTime)}"></span></li>
    <li>${#temporals.monthName(localDateTime)} = <span th:text="${#temporals.monthName(localDateTime)}"></span></li>
    <li>${#temporals.monthNameShort(localDateTime)} = <span th:text="${#temporals.monthNameShort(localDateTime)}"></span></li>
    <li>${#temporals.year(localDateTime)} = <span th:text="${#temporals.year(localDateTime)}"></span></li>
    <li>${#temporals.dayOfWeek(localDateTime)} = <span th:text="${#temporals.dayOfWeek(localDateTime)}"></span></li>
    <li>${#temporals.dayOfWeekName(localDateTime)} = <span th:text="${#temporals.dayOfWeekName(localDateTime)}"></span></li>
    <li>${#temporals.dayOfWeekNameShort(localDateTime)} = <span th:text="${#temporals.dayOfWeekNameShort(localDateTime)}"></span></li>
    <li>${#temporals.hour(localDateTime)} = <span th:text="${#temporals.hour(localDateTime)}"></span></li>
    <li>${#temporals.minute(localDateTime)} = <span th:text="${#temporals.minute(localDateTime)}"></span></li>
    <li>${#temporals.second(localDateTime)} = <span th:text="${#temporals.second(localDateTime)}"></span></li>
    <li>${#temporals.nanosecond(localDateTime)} = <span th:text="${#temporals.nanosecond(localDateTime)}"></span></li>
</ul>

```

```java
    @GetMapping("/date")
    public String date(Model model) {
        model.addAttribute("localDateTime", LocalDateTime.now());
        return "basic/date";
    }
```

> JAVA8의 LocalDateTime을 다루는 많은 방법들

![image](https://user-images.githubusercontent.com/37995817/165956178-577bdc59-1356-415a-93ad-66187b474495.png)



### URL 링크다는 방법

기본적으로 "@{}"사이에 변수던, path던 입력하면 된다.

> 화면 

```html

<ul>
    //일반 URL
    <li><a th:href="@{/hello}">basic url</a></li>
    
    //QueryParam
    <li><a th:href="@{/hello(param1=${param1}, param2=${param2})}">hello query param</a></li>

    //PathVariable
    <li><a th:href="@{/hello/{param1}/{param2}(param1=${param1}, param2=${param2})}">path variable</a></li>

    //PathVariable + QueryParam
    <li><a th:href="@{/hello/{param1}(param1=${param1}, param2=${param2})}">path variable + query parameter</a></li>
</ul>

```

> Controller

```java
    @GetMapping("link")
    public String link(Model model) {
        model.addAttribute("param1", "data1");
        model.addAttribute("param2", "data2");
        return "basic/link";
    }
```

* Controller에서 data를 받아 PathVariable을 구현할 수도 있고, {변수}를 선언하지 않고 추가적으로 붙이면 QueryParam으로 붙는다.

> 링크가 생성된 모습

![image](https://user-images.githubusercontent.com/37995817/165963203-6b175457-7284-4e4e-8756-6f9b023b5d69.png)

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


### 리터럴 (고정 문자 그대로 보여주기)

기본적으로 공백 없이 이어서 붙이면 타임리프가 그대로 보여주는데, 공백이나 특수문자가 생길 경우가 문제가 된다.


> 화면

```html
<ul>
    
    <li>'hello' + ' world!' = <span th:text="'hello' + ' world!'"></span></li>
    <li>'hello world!' = <span th:text="'hello world!'"></span></li>
    <li>'hello ' + ${data} = <span th:text="'hello ' + ${data}"></span></li>
    <li>리터럴 대체 |hello ${data}| = <span th:text="|hello ${data}|"></span></li>
</ul>
```

* 요즘은 주로 리터럴 대체문자로 || 내부에 편리하게 이어서 작성하기가 사용된다.
* 단순히 ""만 사용하면 Pasing 에러가 나므로 숙지해두길!



### 타임리프 기본 연산

* 내부에서 기본적인 비교, 연산은 가능하다.

> 기본 연산

```html
<li>산술 연산
        <ul>
            <li>10 + 2 = <span th:text="10 + 2"></span></li>
            <li>10 % 2 == 0 = <span th:text="10 % 2 == 0"></span></li>
        </ul>
    </li>
    <li>비교 연산
        <ul>
            <li>1 > 10 = <span th:text="1 &gt; 10"></span></li>
            <li>1 gt 10 = <span th:text="1 gt 10"></span></li>
            <li>1 >= 10 = <span th:text="1 >= 10"></span></li>
            <li>1 ge 10 = <span th:text="1 ge 10"></span></li>
            <li>1 == 1 = <span th:text="1 == 10"></span></li>
            <li>1 != 1 = <span th:text="1 != 10"></span></li>
        </ul>
    </li>
```

> 조건부 출력


* 기본 삼항연산자

```html
   <li>조건식
        <ul>
            <li>(10 % 2 == 0)? '짝수':'홀수' = <span th:text="(10 % 2 == 0)? '짝수':'홀수'"></span></li>
        </ul>
    </li>
```
* Elvis 연산자

```html
    <li>Elvis 연산자
        <ul>
            <li>${data}?: '데이터가 없습니다.' = <span th:text="${data}?: '데이터가 없습니다.'"></span></li>
            --null이면 오른쪽 '데이터가 없습니다' 가 출력된다.
            <li>${nullData}?: '데이터가 없습니다.' = <span th:text="${nullData}?: '데이터가 없습니다.'"></span></li>
        </ul>
    </li>
```

* No-Operation

`_` 을 입력하여 null이 들어오면 해당 태그를 타임리프를 무효화하고 기본 html이 출력되게 한다.

```html
    <li>No-Operation
        <ul>
            <li>${data}?: _ = <span th:text="${data}?: _">데이터가 없습니다.</span></li>
            <li>${nullData}?: _ = <span th:text="${nullData}?: _">데이터가 없습니다.</span></li>
        </ul>
    </li>
```


* Controller에서 값 받아서 조건문 생성하기

```html
    <li>test
        <ul>
            <li>${test1} >  ${test2}<span th:text="${test1} > ${test2}"></span></li>
        </ul>
    </li>
```

```java
    @GetMapping("/operation")
    public String operation(Model model) {
        model.addAttribute("nullDate", null);
        model.addAttribute("data", "Spring!");
        model.addAttribute("test1", 1);
        model.addAttribute("test2", 2);

        return "basic/operation";
    }
```

html이 1>2 false로 잘 출력된 모습
![image](https://user-images.githubusercontent.com/37995817/165972426-9e3e2b44-4b34-459a-bf47-9ea3250d2dbb.png)


### Attribute 설정,추가 

* 속성 설정

> 화면

* 그냥 th가  덮어쓴다.

```html
<input type="text" name="mock" th:name="userA" />
<input type="text" name="mock" th:name="${data}" />
```

![image](https://user-images.githubusercontent.com/37995817/165979903-584846ca-5de6-460f-9df6-ea9b3be34edd.png)

* 속성 추가

> 화면

* class 명의 앞, 뒤에 추가

```html
//띄어쓰기 신경
- th:attrappend = <input type="text" class="text" th:attrappend="class=' large'" /><br/>
- th:attrprepend = <input type="text" class="text" th:attrprepend="class='large '" /><br/>
//띄어쓰기 알아서 해줌
- th:classappend = <input type="text" class="text" th:classappend="large" /><br/>
```

![image](https://user-images.githubusercontent.com/37995817/165980206-4ae5fb26-e7e0-4e7a-b13d-68aad8a40be9.png)


### checkbox 체크, 해제 처리

> 화면

* html은 값과 상관없이 checked 속성만 있으면 checkbox를 check한다.
* 타임리프의 th:checked는 false인 경우 checked 속성을 제거한다.

* Controller에서 isChecked 불린 값을 넘겨서 처리하면 되므로 개발자가 개발하기 편해진다.

```html
- checked o <input type="checkbox" name="active" th:checked="${isChecked}" /><br/>
- checked x <input type="checkbox" name="active" th:checked="${isChecked}" /><br/>

- checked=false <input type="checkbox" name="active" checked="false" /><br/>
```

![image](https://user-images.githubusercontent.com/37995817/165980316-e98a1287-7abe-4d7b-ba37-935f642b85e2.png)

//


### if, unless 조건

> 화면

* 조건을 만족하지 않으면 태그 자체를 무효화한다.
* 예제에서는 유저 나이가 

```html
 <tr th:each="user, userStat : ${users}">
        <td th:text="${userStat.count}">1</td>
        <td th:text="${user.username}">username</td>
        <td>
            <span th:text="${user.age}">0</span>
            <span th:text="'미성년자'" th:if="${user.age lt 20}"></span>  20살 lessthan
            <span th:text="'미성년자'" th:unless="${user.age ge 20}"></span> 20살 이상이 아니면
        </td>
    </tr>
```
