# Spring Framework

> 자바 플랫폼을 위한 오픈 소스 애플리케이션 프레임워크

* 동적인 웹 사이트를 개발하기 위한 여러가지 서비스를 제공한다

<br>

> ### Java Platform
> > 프로그램 실행을 위한 하드웨어와 소프트웨어가 결합된 환경
> * 알맞는 운영체제 설치가 필요함 -> 그 운영체제 위에서 자바 프로그램을 실행한다
> * Java Platform = Java VM + Java API
> * Java VM(Virtual Machine) : Java 프로그램 실행 환경을 제공하는 구동엔진
> * Java API(Application Programming Interface) : 프로그램 개발에 필요한 클래스 라이브러리

<br>

# Spring Framework : 특징

1. 경량 컨테이너로서 자바 객체를 직접 관리한다
2. Plain Old Java Object 방식의 프래임워크 : 기존에 존재하는 라이브러리 등을 지원하기에 용이하고 객체가 가볍다
    - Plain Old Java Object, POJO : 오래된 방식의 간단한 자바 오브젝트
    - Java EE 등 중량 프레임워크들을 사용하게 되면서 해당 프레임워크에 종속된 **무거운 객체**를 만들게 된 것에 반발해서 사용된 용어
3. Inversion of Control : 제어권이 사용자가 아닌 프레임워크에 있어서 필요에 따라 스프링에서 사용자의 코드를 호출한다
4. Dependency Injection : 각각의 계층이나 서비스들 간에 의존성이 존재할 경우 프레임워크가 서로 연결시켜준다
5. Aspect-Oriented Programming : 이를 통해서 얻을 수 있는 이점은 다음과 같다
    - 여러 모듈에서 공통적으로 사용하는 기능의 경우 해당 기능을 분리하여 관리할 수 있다
    - 트랜잭션, 로깅 등
6. 영속성과 관련된 다양한 서비스를 지원한다
    - iBATIS 나 Hibernate 등 완성도가 높은 데이터베이스 처리 라이브러리와 연결할 수 있는 인터페이스를 제공한다
7. 확장성이 높다
    - 스프링 프레임워크에 통합하기 위해 간단하게 기존 라이브러리를 감싸는 정도로 스프링에서 사용 가능하다
    - 많은 라이브러리가 이미 스프링에서 지원되고 있으며, 스프링에서 사용되는 라이브러리를 별도로 분리하기도 용이하다

<br>

# Spring Framework : 주요 모듈

### 제어 역전 컨테이너, IoC Container

> 자바의 반영을 이용해서 객체의 생명 주기를 관히라고 의존성 주입을 통해 각 계층이나 서비스들간의 의존성을 맞춘다

원래 이러한 기능들은 주로 환경설정을 담당하는 XML 파일에 의해 설정되고 수행되었으나 현재는 Auto Configuration 을 사용한 YML/YAML 로 구성하기도 한다

* XML 로 설정하는 경우 : 영속성 컨텍스트를 설정하기 위해 XML 형식을 사용해본 적이 있다
    * 하지만 YML 형식으로 변경하였다

```xml
<?xml version="1.0" encoding="UTF-8"?>
<persistence version="2.2"
             xmlns="http://xmlns.jcp.org/xml/ns/persistence" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
             xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/persistence http://xmlns.jcp.org/xml/ns/persistence/persistence_2_2.xsd">
    <!--  EntityManagerFactory 생성 시 사용되는 persistence name -->
    <persistence-unit name="diary">
        <properties>
            <class>com.selab.todo.entity.Diary</class>
            <!-- 필수 속성 -->
            <property name="javax.persistence.jdbc.driver" value="com.mysql.cj.jdbc.Driver"/>
            <property name="javax.persistence.jdbc.user" value="데이터베이스 아이디"/>
            <property name="javax.persistence.jdbc.password" value="비밀번호"/>
            <property name="javax.persistence.jdbc.url" value="데이터베이스 주소"/>

            <!-- 하이버네이트 사용 시 다른 DB에서 MySQL 문법을 사용 가능하도록 변경.-->
            <property name="hibernate.dialect" value="org.hibernate.dialect.MySQL8Dialect"/>
            <!-- 콘솔에 SQL 출력 여부 -->
            <property name="hibernate.show_sql" value="true"/>
            <!-- 가독성 높여주는 formatting 여부 -->
            <property name="hibernate.format_sql" value="true"/>
            <!-- Comment 확인 여부 -->
            <property name="hibernate.use_sql_comments" value="true"/>
        </properties>
    </persistence-unit>
</persistence>
```

### 관점 지향 프로그래밍 프레임워크, Aspect-Oriented Programming Framework

> 스프링은 핵심적인 비즈니스 로직과 관련이 없으나 여러 곳에서 공통적으로 쓰이는 기능을 분리하여 개발하고 실행 시에 서로 조합할 수 있는 관점 지향 프로그래밍을 지원한다

기존에 널리 사용되고 있는 강력한 AOP 인 AspectJ 도 내부적으로 사용할 수 있으며, 스프링 자체적으로 지원하는 Runtime 에 조합하는 방식도 지원한다

```java

@Configuration
public class AppConfig {
    @Bean
    public Service serviceSampleOne(){
        return new Service(algorithmOne, basicAlgorithm);
    }

   @Bean
   public Service serviceSampleTwo(){
      return new Service(algorithmTwo, basicAlgorithm);
   }

   @Bean
   public Algorithm algorithmOne(){
      return new AlgorithmOne();
   }
   
   @Bean
   public Algorithm algorithmTwo(){
      return new AlgorithmTwo();
   }

   @Bean
   public BasicAlgorithm basicAlgorithm(){
        return new BasciAlgorithm();
   }
}
```
이와 같이 Configuration 을 작성한 후 코드에서 원하는 빈을 사용할 수 있다

### 데이터 액세스 프레임워크, Data Access Framework

> 스프링은 데이터베이스에 접속하고 자료를 저장 및 읽어오기 위한 여러가지 유명한 라이브러리가 있으며 이를 통해 데이터베이스 프로그래밍을 쉽게 사용할 수 있다

* 유명한 라이브러리의 예시 : JDBC, iBATIS(MyBatis), Hibernate

### 트랜잭션 관리 프레임워크, Transaction Management Framework

> 스프링인 추상화된 트랜잭션 관리를 지원하며 XML 등을 이용한 선언적 방식 및 프로그래밍을 통한 방식을 모두 지원한다

### 모델-뷰-컨트롤러 패턴, Model-View-Controller Pattern

> 스프링은 웹 프로그래밍 개발 시 거의 표준적인 방식인 Spring MVC 라 불리는 패턴을 사용한다

* DispatcherServlet 이 Controller 역할을 담당하여 각종 요청을 적절한 서비스에 분산시킨다
* 분산된 요청들을 각 서비스들이 처리하여 결과를 생성한다
* 결과는 다양한 형식의 View 서비스들로 화면에 표시될 수 있다

### 배치 프레임워크, Batch Framework

> 스프링은 특정 시간대에 실행하거나 대용량의 자료를 처리하는데 쓰이는 일괄 처리(Batch Processing)을 지원하는 배치 프레임워크를 제공한다

기본적으로 스프링 배치는 Quartz 기반으로 동작한다

### 스프링 부트, Springboot

> 스프링 부트 확장 기능은 독립적인 운영 등급의 스프링 기반 애플리케이션을 만들기 위한 "설정보다 관습" 솔루션이다

* 설정보다 관습 : 프레임워크를 사용하는 개발자가 유연성을 잃지 않고 취해야 하는 결정의 수를 줄이기 위해 시도하는, 소프트웨어 프레임워크에 사용되는 디자인 패러다임

<br>

- - -

* [Spring Framework](https://ko.wikipedia.org/wiki/%EC%8A%A4%ED%94%84%EB%A7%81_%ED%94%84%EB%A0%88%EC%9E%84%EC%9B%8C%ED%81%AC)
* [설정보다 관습](https://ko.wikipedia.org/wiki/%EC%84%A4%EC%A0%95%EB%B3%B4%EB%8B%A4_%EA%B4%80%EC%8A%B5)
* [POJO](https://ko.wikipedia.org/wiki/Plain_Old_Java_Object)