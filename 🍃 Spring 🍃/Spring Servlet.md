# Servlet

> 웹 프로그래밍에서 클라이언트 요청을 처리하고 처리 결과를 클라이언트에 전송하는 기술.
> 자바로 구현된 CGI(Common Gateway Interface).

* CGI : 별도로 제작된 서버와 프로그램 간의 교환 방식. HTML 의 GET, POST 방법으로 클라이언트의 데이터를 환경변수로 전달하고 프로그램의 표준 출력 결과를 클라이언트에 전송하는 것

```
자바를 사용해서 웹을 만들기 위해 필요한 기술
```

### 특징

* 클라이언트 요청에 대해 동적으로 작동하는 웹 어플리케이션 컴포넌트
* HTML 을 사용해서 요청에 응답한다
* Java Thread 를 통해 동작한다
* MVC 패턴 중 Controller 로 이용된다
* HTTP 프로토콜 서비스를 지원하는 `javax.servlet.http.HttpServlet` 클래스를 상속받는다
    * UDP 보다 속도가 느리다
* HTML 변경시 Servlet 을 다시 컴파일 해야한다.

```java
// servlet-url 로 요청이 오면 이게 실행된다.
@WebServlet(name = "sampleServlet", urlPatterns = "/servlet-url")
public class SampleServlet extends HttpServlet {

    // ctrl + o로 protected(자물쇠 표시)인 메서드를 오버라이드 한다.
    @Override
    protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        System.out.println("SampleServlet.service");
        System.out.println("request = " + request);
        System.out.println("response = " + response);
    }
}
```

#### WebServlet 어노테이션

* name : 서블릿 이름
* urlPatterns : URL 매핑
* 옵션들은 값이 겹치면 안 된다

# HttpServletRequest

HTTP 요청을 개발자가 직접 파싱하는 것은 귄찮다. 그래서 HttpServletRequest 객체에 담아서 결과를 제공한다.

```
POST /save HTTP/1.1
Host: localhost:8080
Content-Type: application/x-www-form-urlencoded

username=kim&age=20
```

#### start line

* http method
* url
* query string
* schema, protocol

#### header

* host
* content-type

#### body

* form parameter
* message body 등에 데이터 조회

<br>

`HttpServletRequest` 를 사용하면 위와 같은 HTTP 요청 메시지를 편리하게 조회할 수 있다

## 임시 저장소 긴으

* 해당 HTTP 요청의 시작부터 끝까지 유지되는 임시 저장소 기능
* 저장 : request.setAttribute(name, value)
* 조회 : request.getAttribute(name, value)

## 세션 관리 기능

* 세션을 이용해 로그인을 유지할 수 있다
* request.getSession(create:true)

# HttpServletResponse

* HTTP 응답 메시지를 생성하는 역할을 한다
* HTTP 응답 코드를 지정한다
* 헤더와 바디를 생성한다
* 편의 기능을 제공한다 : content-type, 쿠기, redirect

```java
package hello.servlet.basic.response;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.Cookie;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.io.PrintWriter;

@WebServlet(name = "responseHeaderServlet", urlPatterns = "/response-header")
public class ResponseHeaderServlet extends HttpServlet {

    @Override
    protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // start line
        response.setStatus(HttpServletResponse.SC_OK);

        // headers
        response.setHeader("Content-Type", "text/plain;charset=utf-8");
        response.setHeader("Cache-Control", "no-cache, no-store, must-revalidate");
        response.setHeader("Pragma", "no-cache");
        // 임의의 헤더 생성 가능
        response.setHeader("my-header", "hello");

        // 편의 메서드
        content(response);
        cookie(response);
        redirect(response);

        // message body
        PrintWriter writer = response.getWriter();
        writer.print("ok");
    }

    private void content(HttpServletResponse response) {
        /*
        Content-Type: text/plain;charset=utf-8
        Content-Length: 2
        response.setHeader("Content-Type", "text/plain;charset=utf-8");
        */
        response.setHeader("Content-Type", "text/plain;charset=utf-8");
        response.setContentType("text/plain");
        response.setCharacterEncoding("utf-8");
        // 생략 시 자동 생성
        // response.setContentLength(2);
    }

    private void cookie(HttpServletResponse response) {
        /*
        Set-Cookie: myCookie=good; Max-Age=600;
        response.setHeader("Set-Cookie", "myCookie=good; Max-Age=600");
        */
        Cookie cookie = new Cookie("myCookie", "good");
        cookie.setMaxAge(600); //600초
        response.addCookie(cookie);
    }

    private void redirect(HttpServletResponse response) throws IOException {
        /*
        Status Code 302
        Location: /basic/hello-form.html
        */
        response.setStatus(HttpServletResponse.SC_FOUND);
        response.setHeader("Location", "/basic/hello-form.html");
        response.sendRedirect("/basic/hello-form.html");
    }
}
```

## 응답 방법

* 텍스트
* HTML
* JSON
