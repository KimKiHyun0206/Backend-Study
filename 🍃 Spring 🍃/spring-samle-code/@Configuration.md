# @Configuration

> `@Configuraition` 어노테이션은 스프링 `IOC Container`에게 해당 클래스를 Bean 구성 Class임을 알려주는 것이다

### @Bean | @Component

> `@Bean` 어노테이션와 `@Component` 어노테이션 둘 다 Spring(IOC) Container에 Bean을
> 등록하도록 하는 메타데이터를 기입하는 어노테이션이다. 그렇다면 왜 두 개나 만들었을까
> > 정담 : 둘의 용도가 다르기 때문

#### @Bean

> @Bean 어노테이션의 경우 개발자가 직접 제어가 불가능한 외부 라이브러리 등을 Bean으로
> 만들 때 사용된다.

```java

@Configuration
public class ApplicationConfig {
    @Bean
    public ArrayList<String> array() {
        return new ArrayList<String>();
    }
}
```

이는 @Bean 어노테이션을 이용하여 Bean을 생성한 예제이다.
> 위와 같이 `ArrayList` 같은 라이브러리 등을 Bean으로 등록하기 위해서는
> 별도로 해당 라이브러리 **`객체를 반환하는 Method`**를 만들고
> `@Bean` 어노테이션을 붙여주면 된다.
> 위의 경우 Bean어노테이션에 아무런 값을 지정하지 않았으므로 Method 이름을
> CamelCase로 변경한 것이 Bean id로 등록된다
> > 메소드 이름이 arrayList()인 경우 `arrayList`가 `Bean id` 이다.

```java

@Configuration
public class ApplicationConfig {
    @Bean(name = "myarray")
    public ArrayList<String> array() {
        return new ArrayList<String>();
    }
}
```

위와 같이 @Bean 어노테이션에 name이라는 값을 이용하면 자신이 원하는 id로 Bean을 등록할 수 있다.

의존관계가 필요할 때는 어떻게 해결할 수 있을까?

```java
public class ApplicationConfig {

    @Bean
    public ArrayList<String> array() {
        return new ArrayList<String>();
    }

    @Bean
    public Student student() {
        return new Student(array());
    }
}   
```

> Student 객체의 경우 생성자에서 ArrayList를 주입 받도록 코드를 짜놓았다. 이럴 때에는 `Bean`으로
> 선언된 `array()`메소드를 호출함으로써 의존성을 주입할 수 있다.

#### @Component

> @Component 어노테이션은 개발자가 직접 작성한 Class를 Bean으로 등록하기 위한 어노테이션이다

```java

@Component
public class Student {
    public Student() {
        System.out.println("hi");
    }
}
```

`Student` 클래스는 개발자가 사용하기 위해서 직접 작성한 클래스이다. 이러한 클래스를 `Bean`으로
등록하기 위해 상단에 `@Component` 어노테이션을 사용한다

```java

@Component(value = "mystudent")
public class Student {
    public Student() {
        System.out.println("hi");
    }
}
```

이 역시 아무런 정보다 없다면 클래스 이름을 `CamelCase`로 변경한 것이 `Bean id`로 사용된다

#### @Autowired

> @Component를 사용한 Bean의 의존성 주입은 @autowired 어노테이션을 이용하여 할 수 있다.
> 위와 같이 Student가 Pencil에 대한 의존성을 가지고 있는 경우 @Autowired 어노테이션을
> 이용하여 의존성을 자동으로 주입할 수 있다. 이때 당연히 Pencil도 @Component 어노테이션을
> 가지고 있어야 한다. 그래야만 IOC Container에 Bean으로 등록되기 때문이다.

```java

@Component
public class Pencil extends Write {

}

@Component
public class People {
    @Qualifier("pencil")
    @Autowired
    private Write write;
}
```

`@Autowired` 어노테이션의 경우 `형(타입)`을 통해 해당 자리에 들어올 객체를 판별하여 주입한다.
따라서 해당 자리에 들어올 수 있는 객체가 여러 개인 경우, 즉 다형성을 띄는 객체 타입에
`@Autowired`를 사용한 경우에는 `@Bean("Bean이름")`을 이용하여 해당 자리에 주입될 Bean을 명시해야 한다
> 위의 클래스에서는 Pencil 이 Write 를 상속하기 때문에 `@Qualifier("pencil")`을 사용했다.

### 사용 방법

#### @Bean

```java
public class Student {
    public Student() {
        System.out.println("hi");
    }
}
```

우선 의존성 주입 대상 클래스를 생성한다

```java

@Configuration
public class ApplicationConfig {
    @Bean
    public Student student() {
        return new Student();
    }
}
```

`Student`를 `Bean`으로 등록하기 위해 `Config Class`를 임의로 만들고 `@Configutation` 어노테이션을 부여한다.

```java
public class Main {
    public static void main(String[] args) {
        ApplicationContext contect =
                new AnnotationConfigApplicationContext(ApplicationConfig.class);
        Student student = context.getBean("student", Student.class);
    }
}
```

Annotation 을 기반으로 Bean을 등록했으므로 `AnnotationConfigApplicationContext`객체를
생성하고 매개 변수로 `@Configuration` 어노테이션을 부여한 `ApplicationConfig` 클래스를 넘겨준다.
이후 getBean을 사용하면 된다

#### @Component

> `@Component` 어노테이션이 부여된 클래스들은 자동으로 `IOC Container`에 Bean으로 등록이 된다.
> `IOC Contatiner`에게 이러한 어노테이션이 부여된 클래스를 자동으로 `Bean`으로 등록하라고 하기 위해서 XML 파일에 따로 설정이 필요하다

```xml

<xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns="http://www.w3.org/2001/XMLSchema-instance"

       xmlns:context="http://www.springframework.org/schema/context"

       xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
    http://www.springframework.org/schema/context/spring-context-3.2.xsd">

<context:component-scan base-package="com.java.sample">
</context:component-scan>

</bean>
```

다시 Source 탭으로 돌아와 `<context:component-scan base-package="com.java.sample></component:component-scan>` 코드를 추가해주면 준비가
완료된다.

```java

@Component(value = "mystudent")
public class Student {
    public Student() {
        System.out.println("hi");
    }
}
```

`@Component` 어노테이션을 부여한 Student 클래스이다

```java
public class Main {
    public static void main(String[] args) {
        ApplicationContext context =
                new ClassPathXmlApplicationContext("scan.xml");
        Student student = context.getBean("mystudent", Student.class);
    }
}
```

Main 클래스에서는 기존 XML을 이용하여 의존성을 주입하듯 객체를 생성하면 된다

### 요약

* `@Component`는 개발자가 직접 작성한 클래스를 `Bean`으로 만드는 것
* `@Bean`은 개발자가 작성한 `Method`를 통해 반환되는 객체를 `Bean`으로 만드는 것
* 각자의 용도가 정해져 이으므로 정해진 곳에서만 사용이 가능하다