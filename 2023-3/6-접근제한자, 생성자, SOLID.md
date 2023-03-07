# 접근제한자

```java
public class Sample {
    private int a;
    int b;
    protected int c;
    public int d;
}
```

객체지향 Java에는 위처럼 접근제한자가 있다.

## 용도

> 다른 클래스에서 자신에 대한 접근을 막기 위해 사용한다

객체지향을 제대로 설계하기 위해서는 캡슐화를 해야 한다. 이때 캡슐화를 하기 위하여 접근 제한자가 필요하다.

### 캡슐화

> 객체의 자율성을 보장하기 위해 지켜야 하는 첫 번째 단계.

클래스 내부를 다른 클래스가 모르게 하게 만드는 것이다.

```java

@Getter
class People {
    private String name;
    private Integer age;
}
```

이런식으로 코드를 작성하게 되면 객체 내부의 데이터에 접근하기 위해서는 getter 메소드를 사용해야만 한다.

```
물론 위의 코드는 잘못되었다.
객체는 단순한 데이터 제공자가 아니기 때문이다.
```

# 생성자

> 클래스를 인스턴스로 만들기 위한 메소드.

```java
class People {
    private String name;
    private Integer age;

    People(String name, Integer age) {
        this.name = name;
        this.age = age;
    }
}
```
위처럼 모든 변수에 값을 할당하는 방법도 있고 일부분만 값을 할당하여 만드는 방법도 있다.
```java
class People {
    private String name;
    private Integer age;

    People(String name) {
        this.name = name;
    }
    
    People(){
    }
}
```
스프링에서는 생성자를 통해 의존성을 주입받는 방법이 있는데 이는 객체지향의 인스턴스 생성의 대표적인 예이다.

# SOLID
> 객체지향의 설계 원칙을 5가지로 분류해놓은 것

## SCP, 단일 책임 원칙
## OCP, 계방 폐쇄 원칙
## LSP, 리스코프 치환 원칙
## ISP, 인터페이스 단일 책임 원칙
## DIP, 의존성 역전 원칙