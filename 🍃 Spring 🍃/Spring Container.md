# Spring Container

> 💡 **Spring IOC Container 라고도 하며 Bean 들의 LifeCycle 을 관리한다**

* Bean 의 생성, 관리, 제거 등의 역할 담당
* Spring Framework 에서 IOC 를 사용하는 이유가 Spring Bean 들의 생명주기를 관리하기 위해서이다

<br>

# Bean 을 관리하는 과정

1. DI 가 이루어진 Bean 들을 BeanFactory 와 ApplicationContext 라는 2 개의 컨테이너로 제어하고 관리한다
2. BeanFactory 와 ApplicationContext 는 인터페이스로 각 구현체가 여러 개가 있다
3. 이 구현체를 사용하는 경우는 우리가 MVC 패턴에 맞추어 코드를 작성하고 Test 하기 위함이다

<br>

# 왜 Spring Container 를 사용하는가?

> 💡 **좋은 객체지향 설계를 하고, 객체 간의 의존성을 낮추기 위해 Spring Container 를 사용한다**

* 일반적으로 코드를 작성할 때 자바는 new 연산자를 사용하게 된다
* 이때 new 연산자를 사용하게 되면 의존성을 높이는 원인이 된다
* 이는 DIP 를 위배하게 되고, 객체지향 설계 원칙에 어긋난다

![img.png](../🔲%20Image%20🔲/Spring/SpringContainer-ApplicationContext.png)
ApplicationContext 인터페이스가 다른 인터페이스들을 다중 상속한다

|          인터페이스명           | 기능                           |
|:-------------------------:|:-----------------------------|
|       MessageSource       | 다국어 메시지 처리 기능 제공             |
|    EnvironmentCapable     | 프로파일 기능, 프로퍼티 기능들 제공         |
|        BeanFactory        | 스프링 빈을 관리하고 조회하는 역할 담당       |
| ApplicationEventPublisher | 이벤트 기반의 프로그래밍을 할 때 필요한 기능 제공 |
|      ResourceLoader       | 리소스를 읽어오는 기능을 제공             |

> 💡 **BeanFactory 기능을 모두 상속해 Bean 객체를 관리하며, 메시지 처리, 리소스, 이벤트와 관련된 기능을 추가적으로 가지고 있는 인터페이스**

* 하지만 BeanFactory 와 ApplicationContext 의 기능은 비슷하지만 ApplicationContext 의 기능이 더 많다
* 보편적으로 사용되는 것은 ApplicationContext 이다

<br>

# BeanFactory 와 ApplicationContext 의 Bean 생성 시기 차이점

|  비교   | BeanFactory                   |         ApplicationContext          |
|:-----:|:------------------------------|:-----------------------------------:|
| 생성 시기 | 미리 생성하지 않는다                   | App 을 실행할 때 존재하는<br/>Context 초기화 시점 |
| 생성 방법 | getBean() 을 사용하여 호출된 시점에 빈 생성 |      모든 Singleton Bean 을 생성한다       |
|  장점   | 메모리를 아낄 수 있다                  |         지연 없이 서비스를 제공할 수 있다         |

* Singleton 객체가 사라지는 시점은 Application 이 종료되는 시점이다

<br>

# Java Bean 과 Spring Bean

* **Bean** : 애플리케이션에서 사용하는 객체
* **Java Bean** : 단순히 클래스에서 Getter | Setter 만 가진 클래스(객체)
* **Spring Bean** : Spring Container 가 관리하는 자바 객체

<br>

# Spring Container 사용 방법

1. ApplicationContext 의 구현체인 AnnotationConfigApplicationContext 와 @Configuration 이 적용되어 있는 클래스 파일을 이용해서 객체를 만들어야 한다

```java

@Controller
@RequiredArgsConstructor
public class Controller {
    //이렇게 선언해 두변 Spring Container 가 알아서 생성해서 넣어준다
    private final AnnotationConfigApplicationContext app;

    public Response register(Long id) {
        MainLogic mainLogic = app.getBean("mainLogic");
        var response = mainLogic.register(id);
        return Response.from(response);
    }
}

@Configuration
public class AppConfig {

    @Bean
    public MainLogic mainLogic() {
        return new MainLogic();
    }
}
```

2. Spring Container 에 등록된 빈들을 가지고 와서 쓰려면 getBean 메소드를 이용해서 가져와서 사용하면 된다.
3. 이때 getBean()으로 가져오기 위해서는 빈의 이름을 알아야 하기 때문에 주도권이 개발자에게 가게 되어 IOC 가 깨진다
    1. getBean()은 정말 어쩔 수 없는 경우에만 쓰인다

## 수동 등록

> 💡 **위에서 본 방식을 수동 등록이라고 한다**

> @Configuration + @Bean

* 수동 등록을 하면 빈 객체를 여러 형식으로 조립하여 개발자가 직접 넣어줄 수 있다
* 자동으로 하는 것보다 불편하지만 객체가 어떻게 만들어지는지 볼 수 있다
* 싱글톤을 보장하여 객체를 생성할 수 있다
*

## 자동 등록

> 💡 **클래스에 @Component 가 붙은 어노테이션을 붙여서 사용한다**

> @ComponentScan + @Component

```java

@Service
public class UserLogic {
   ...
}

@Repository
public interface UserRepository {
   ...
}

@Controller
public class UserController {
   ...
}
```

* 위의 세 어노테이션은 모두 @Component 를 상속 받는다
* 따라서 자동으로 빈을 생성할 수 있다

### @Component

> 💡 **Class 레벨에서 선언함으로써 스프링이 Runtime 에 ComponentScan 을 하여 자동으로 빈을 찾고 등록하는 어노테이션**

* 찾다 : Detect

```java

@Component
public class User {

}
```

### @ComponentScan

> 💡 **해당 클래스의 패키지와 하위 패키지에 있는 @Component 어노테이션이 부여된 Class 를 탐색하여 빈으로 등록해주는 어노테이션**

```java

@ComponentScan
@SpringBootApplication
public class Application {

    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }

}
```

### @Configuration

> 💡 **Bean 메타 정보들을 담고 있는 클래스라고 SpringContainer 에게 알려주는 어노테이션**

* 이 클래스 안에 우리가 관리할 빈들이 있다고 알리는 것
* 이 어노테이션이 설정된 클래스의 빈들이 싱글톤 방식으로 보장되어야 한다는 것을 알리기도 한다

### @Bean

> 💡 **메소드 레벨에서 선언하며, 반환되는 객체(인스턴스)를 개발자가 수동으로 빈으로 등록하는 어노테이션**

* 해당 메소드가 반환하는 객체를 빈으로 등록한다





