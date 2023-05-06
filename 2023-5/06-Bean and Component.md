# Bean

> Spring IOC Container가 관리하는 자바 객체

## 등록 방법

### 자바 어노테이션을 사용하는 방법

```java

@Service
public class TaxService {

}
```

* @Controller
* @Entity
* @Component

등을 사용해서 등록할 수도 있다

<br>

### Bean Configuration File에 직접 등록하는 방법

```java

@Configuration
public class AppConfig {
    @Bean
    public Controller masterController(){
        return new MasterController();
    }
}
```
* 이때 등록은 메소드 이름으로 등록되게 된다
* 이것을 잘 사용하면 다양한 빈 객체를 만들 수 있다

```java
@Configuration
public class AppConfig{
    @Bean
    public GameService easyMode(){
        return new GameService(new EasyMode(), new EastPlayer());
    }
    
    @Bean
    public GameService hardMode(){
        return new GameService(new HardMode(), new HardPlayer());
    }
}
```

<br>

# Component
> 독립적인 단위 모듈, 유저가 사용하는 시스템에 대한 조작장치를 이야기한다
* 개발자가 직접 작성한 `Class`를 `Bean`으로 만드는 것
* 선언적 어노테이션

# Component Scan
> 스프링이 스프링 빈으로 등록될 준비가 된 클래스들을 스캔하여 빈으로 등록해주는 과정
* `@Component`가 붙은 모든 클래스가 대상이 된다
* 우리가 사용하는 대부분의 어노테이션에 상속된다
  * `@Controller`, `@Service` 등

## 범위
```java
@ComponentScan(
        basePaskages = "hello.world"
)
```
* 보통 이렇게 성정하지 않는다
* `@SpringBootApplication`을 프로젝트의 시작 루트에 두는 것이 관례이다
* `@SpringBootApplication`에 `@ComponentScan`이 상속되어있다

