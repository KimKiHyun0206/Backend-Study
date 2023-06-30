# IOC, Inversion of Control = 제어의 역전

> 원래는 개발자가 의존성을 만들어주지만 Spring은 어노테이션을 통해서 의존성을 주입하여
> 제어 권한을 가져간다

```java

@Service
@RequiredArgsConstructor
public class TaxService {
    private final TaxPolicy policy;
    private final PeopleRepository peopleRepository;

    public TaxResponse taxes(Long id, Long amount) {
        ...
    }
}
```

* 스프링은 위처럼 자동적으로 의존성을 주입해줄 수 있다
* 이는 직접적으로 의존성을 만드는 것이 아닌, 외부에서 의존성을 가져오는 것이다

<br>

# DI, Dependency Injection = 의존성 주입

> 밖에서 객체에게 의존성을 주입해주는 것

* DI는 IOC의 일종이라고 생각할 수 있다

### 주입 방법

1. 스프링 IOC 컨테이너가 Bean 객체들을 관리한다
    1. Bean들의 의존성을 관리한다
    2. 객체를 만들어준다
    3. Bean으로 등록해준다
2. ApplicationContext 또는 BeanFactory를 사용하여 Bean을 사용한다
3. 스프링에서 의존성 주입을 하려면 Bean으로 등록된 객체여야 한다
    1. 뭔진 알아야 스프링이 넣어줄 것이다
    2. 어노테이션

```java

@Bean
public class BeanSample {

}
```

<br>

# AOP, Aspect Oriented Programming = 관점 지향 프로그래밍

> 어떤 로직을 기준으로 핵심적인 관점, 부가적인 관점으로 나누어서 보고
> 그 관점을 기준으로 각각 모듈화 하겠다는 의미

* `모듈화` : 공통된 로직이나 기능을 하나의 단위로 묶는 것
* `흩어진 관심사` : 소스 코드에서 다른 부분에 계속 반복해서 쓰이는 코드들

## AOP의 주요 개념

* Aspect : 흩어진 관심사를 모듈화 한 것.
    * 주로 부가기능을 모듈화 한다
* Target : Aspect를 적용한 곳.
    * 클래스, 메소드 등
* Advice : 실질적으로 어떤 일을 해야할 지에 대한 것,
    * 실질적인 부가기능을 담은 구현제
* JointPoint : Advice가 적용될 위치, 끼어들 수 없는 지점.
    * 메소드 호출 지점, 생성자 호출 지점, 필드에서 값을 꺼내올 때
* Point Cut : JointPoint의 상세한 스펙을 정의한 것
    * A란 메소드의 진입 시점에서 호출할 것과 같이 더욱 구체적으로 Advice가 실행될 지점 설정 가능

## 특징

* 프록시 패턴 기반의 AOP 구현체, 프록시 객체를 쓰는 이유는 접근 제어 및 부가기능을 추가하기 위함
* 스프링 빈에만 AOP 적용 가능
* 모든 AOP 기능을 제공하는 것이 아닌 스프링 IOC와 연동하여 엔터프라이즈 애플리케이션에서 가장 흔한 문제에 대한 해결잭을 지원하는 것이 목적
    * 문제 : 중복 코드, 프록시 클래스 작성의 번거로움, 객체들 간 관계 복잡도 증가

## @AOP

```xml

<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-aop</artifactId>
</dependency>
```

* @Aspect 어노테이션을 붙여 이 클래스가 Aspect를 나타내는 클래스라는 것을 명시
* @Component로 스프링 빈으로 등록

```java

@Aspect
@Component
public class Sample {

}
```

이처럼 의존성을 추가하면 다양한 기능을 사용할 수 있다
