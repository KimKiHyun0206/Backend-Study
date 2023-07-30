# MVC 패턴

## 왜 MVC 패턴이 필요할까?

### 너무 많은 역할

* 서블릿이나 JSP 하나로 비즈니스 로직과 뷰 랜더링까지 다 처리하면 너무 많은 역할을 한다
* 이는 유지 보수를 어렵게 만든다

### 변경의 라이프 사이클

* 둘 사이의 변경 라이브 사이클이 다르다
* UI 일부를 수정하는 일과 비즈니스 로직을 수정하는 일은 서로 다르게 발생할 확률이 놓다
* 대부분 서로에게 영향을 주지 않는다

### 기능 특화

* JSP 같은 뷰 템플릿은 화면을 렌더링하는 데에 최적화되어 있다
* 때문에 화면 렌더링만 담당하는 것이 효과적이다

## Model View Controller

### Model

* 뷰에 출력할 데이터를 담는다
* 뷰에 필요한 데이터를 모두 모델에 담아 전달한다
* 뷰는 비즈니스 로직이나 데이터 접근에 대해 몰라도 된다

### VIEW

모델에 담긴 데이터를 사용해 화면을 그리는 일에 집중한다

### Controller

* HTTP 요청을 받아서 파라미터를 검증한다
* 비즈니스 로직을 실행한다
* 뷰에 전달할 결과 데이터를 조회해 모델에 담는다

# Spring MVC

![img.png](../🔲%20Image%20🔲/리펙토링 이전의 이미지/img2/Spring-MVC-Pattern.png)

1. Client 로부터 요청이 들어오면 DispatcherServlet 이 호출된다.
2. DispatcherServlet 은 받은 요청을 HandlerMapping 에게 던져준다. 요청받은 URL 을 분석하여 HandlerMapping 적합한 Controller 를 선택하여 반환한다.
3. DispatcherServlet 는 다음으로 HandlerAdapter 를 호출한다. Handler Adapter 는 해당하는 Controller 중 요청한 URL 에 맞는 적합한 Method 를 찾아준다.
4. Controller 는 Business Logic 을 처리하고, 해당하는 결과를 View 에 전달할 객체를 Model 에 저장한다.
5. Controller 는 View name 을 DispatcherServlet 에게 리턴한다.
6. DispatcherServlet 은 ViewResolver 를 호출하여 Controller 가 리턴한 View name 을 기반으로 적합한 View 를 찾아준다.
7. DispatcherServlet 은 View 객체에 처리결과를 넘겨 최종 결과를 보여주도록 요청한다.
8. View 객체는 해당하는 View 를 호출하며, View 는 Model 객체에서 화면 표시에 필요한 객체를 가져와 화면 표시를 처리하고 Client 에게 넘겨준다.

## DispatcherServlet

* `org.springframework.web.servlet.DispatcherServlet`
* 스프링 MVC도 프론트 컨트롤러 패턴으로 되어있다
* **스프링 MVC의 프론트 컨트롤러이며 스프링 MVC의 핵심**

```
DispatcherServlet - FrameworkServlet - HttpServletBean - HttpServlet
```

* DispatcherServlet 은 HttpServlet 을 상속받아 서블릿으로 동작한다
* 스프링 부투는 DispatcherServlet 을 서블릿으로 자동 등록한다
    * 모든 경로에 대해서 매핑한다
    * 경로가 자세할수록 우선순위가 높다
    * 따라서 기존에 등록한 서블릿도 함께 동작한다

### 요청 흐름

1. 서블릿이 호출되면 HttpServlet 이 제공하는 service()가 호출된다
2. 스프링 MVC 는 DispatcherServlet 의 부모인 FrameworkServlet 에서 service()를 오버라이드 했다
3. FrameworkServlet.service()를 시작으로 여러 메소드가 호출되면서 **DispatcherServlet.doDispatch()**가 호출된다.

```java
public class DispatcherServlet extends FrameworkServlet {
    protected void doDispatch(HttpServletRequest request,
                              HttpServletResponse response) throws Exception {
        HttpServletRequest processedRequest = request;
        HandlerExecutionChain mappedHandler = null;
        ModelAndView mv = null;

        // 1. 핸들러 조회
        mappedHandler = getHandler(processedRequest);
        if (mappedHandler == null) {
            noHandlerFound(processedRequest, response);
            return;
        }

        // 2. 핸들러 어댑터 조회: 핸들러를 처리할 수 있는 어댑터를 찾는다.
        HandlerAdapter ha = getHandlerAdapter(mappedHandler.getHandler());

        // 3. 핸들러 어댑터 실행
        // 4. 핸들러 어댑터를 통해 핸들러 실행
        // 5. ModelAndView 반환
        mv = ha.handle(processedRequest, response, mappedHandler.getHandler());

        processDispatchResult(processedRequest, response, mappedHandler, mv, dispatchException);
    }

    private void processDispatchResult(HttpServletRequest request,
                                       HttpServletResponse response,
                                       HandlerExecutionChain mappedHandler,
                                       ModelAndView mv,
                                       Exception exception) throws Exception {
        // 뷰 렌더링 호출
        render(mv, request, response);
    }

    protected void render(ModelAndView mv,
                          HttpServletRequest request,
                          HttpServletResponse response) throws Exception {
        View view;
        String viewName = mv.getViewName();

        // 6. 뷰 리졸버를 통해서 뷰 조회
        // 7. View 반환
        view = resolveViewName(viewName, mv.getModelInternal(), locale, request);

        // 8. 뷰 렌더링
        view.render(mv.getModelInternal(), request, response);
    }
}
```