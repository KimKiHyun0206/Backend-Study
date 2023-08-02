# 들어가기 앞서서

Java 라는 언어를 사용하다 보면 가끔 상속이나 구현을 사용해야 할 때가 나오는데, 이때 상속 받거나 구현하는 클래스의 특성에 맞게 메소드를 오버로딩 또는 오버라이딩 하는 경우가 있다.

오버로딩과 오버라이딩의 개념을 확실하게 구분하고, 코드로 예시를 들면서 설명하며 개념을 확실히 정리하고, 내 경험을 정리해볼 것이다.

<br>

# Override

> 💡 **부모 클래스로부터 상속받은 메서드의 내용을 재정의(변경) 하는 것**

```java
public class CoffeeDrip {
    public Coffee makeCoffee(Bean bean, Water water) {
        // 원두를 추출하여 new Coffee() 생성
        return coffee;
    }
}

public class HandDrip extends CoffeeDrip {
    @Override
    public Coffee makeCoffee(Bean bean, Water water) {
        // 핸드드립으로 원두를 추출하여 new Coffee() 생성
        return coffee;
    }
}

public class MachineDrip extends CoffeeDrip {
    @Override
    public Coffee makeCoffee(Bean bean, Water water) {
        // 머신드립으로 원두를 추출하여 new Coffee() 생성
        return coffee;
    }
}
```

* 이처럼 상속 관계에서 메소드를 제정의할 때 오버라이딩을 사용한다

## Overriding : 조건

1. 자식 클래스의 오버라이딩 하려는 메소드는 부모 클래스의 메소드와 이름, 매개변수, 반환타입이 같아야 한다
2. 접근 제어자는 조상 클래스의 메소드보다 좁은 범위로 변경할 수 없다
3. 부모 클래스의 메소드보다 많은 수의 예외를 선언할 수 없다
4. 인스턴스 메소드의 static 메소드 또는 그 반대로 변경할 수 없다

<br>

# Overload

> 💡 **자바의 한 클래스 내에 이미 사용하려는 이름과 같은 이름을 가진 메소드가 있더라도 매개변수의 개수 또는 타입이 다르면, 같은 이름을 사용해서 메소드를 정의할 수 있다**

```java
public class Sample {
    public void print() {
        System.out.println("오버로딩1");
    }

    String print(Integer a) {
        System.out.println("오버로딩2");
        return a.toString();
    }

    void print(String a) {
        System.out.println("오버로딩3");
        System.out.println(a);
    }

    String print(Integer a, Integer b) {
        System.out.println("오버로딩4");
        return a.toString() + b.toString();
    }
}
```
* 이처럼 한 클래스 내에서 같은 이름의 메소드를 사용하고 싶을 때 사용한다

## Overload : 조건
1. 매개변수가 다르다면 리턴값을 자유롭게 바꿀 수 있다
2. **매개변수를 자유롭게 바꿀 수 있다**(사실상 매개변수의 차이로만 구현할 수 있다)
3. 내부 동작은 다른 오버로드 된 메소드와 연관되지 않는다

<br>

# 마치며
개념을 확실히 정리하면서 어떤 예시가 있는지 코드로 나타내 보았다. 이번에 오버라이딩에서 오버라이딩 된 메소드는 부모의 메소드보다 더 많은 예외를 처리할 수 없다는 것을 처음 알게 되었다.
그리고 오버로드에 매개변수로만 구현한다는 말이 처음에는 이해가 되지 않았는데, 컴파일러의 입장에서 생각해 보면 리턴 값만 가지고 오버르도 된 함수를 구분하라고 하는 것은 문제가 있다고 생각했다.





