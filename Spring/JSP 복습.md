# JSP, Java Server Pages

> HTML 코드에 Java 코드를 넣어 동적 웹 페이지를 생성하는 웹 애플리케이션 도구

* 실행 시 Java Servlet 으로 변환
* 웹 어플리케이션이 서버에서 동작되면서 필요한 기능 수행
* 수행 후 생성된 데이터를 웹 페이지와 함께 클라이언트로 응답

### Java Servlet

> 자바 언어를 기반으로 만들어지며 웹 어플리케이션 서버 위에서 컴파일 되고 동작한다

* 서블릿 : 웹 페이지를 동적으로 생성하기 위한 서버측 프로그램

### JSP 와 Servlet

|    구분     |         JSP          |           Servlet           |
|:---------:|:--------------------:|:---------------------------:|
| 소스 코드의 위치 | HTML 내부에 Java 코드가 있다 |    자바 코드 내에 HTML 코드가 있다     |
|    장점     |         간편하다         | 웹 애플리케이션에서 동작하는 것은 결국 서블릿이다 |

* JSP 만 안다고 해서 끝나는 것이 아니라 Servlet 도 공부해야 한다
* JSP 로 작성된 프로그램은 서버로 요청시 Servlet 파일로 변환되어 JSP 태그를 분해하고 추출하여 다시 순수한 HTML 을 반환한다

### 동작 순서

1. `Client` : REQUEST - hello.jsp
2. `JSP Container` : READ - hello.jsp
3. `JSP Container` : GENERATE(변환) - Servlet.java 파일 생성
4. `Servlet.java` : COMPILE - Servlet.class
5. `Servlet.class` : EXECUTE - JSP Container 에게 HTML 파일 생성하여 전달
6. `JSP` : HTTP Protocol 을 통해 HTML 페이지를 Client 에게 전달한다

