# Object Class

## java.lang 패키지
* 자바에서 가장 기본적인 동작을 수행하는 클래스들의 집합
* import 문을 사용하ㅣㅈ 않아도 클래스 이름 만으로 바로 사용 가능

## java.lang.Object 클래스
* 모든 자바 클래스의 최고 조상 클래스
* 자바의 모든 클래스는 Object 클래스의 모든 메소드를 바로 사용할 수 있다
* 필드를 가지지 않으며 총 11개의 메소드만으로 구성되어있다

## Method
### toString()
* 해당 인스턴스에 대한 정보를 문자열로 반환한다
* 이때 반환되는 문자열은 클래스 이름과 함께 구분자로 `@`가 사용된다
* 그 뒤로 16진수 해시 코드가 추가된다
* 해시 코드는 인스턴스의 주소를 가리키는 값으로 인스턴스마다 다르게 반환된다

```java
public class ToStringSample{
    public static void main(String[] args) {
        System.out.println(new Object.toString());
    }
}
```

<br>

### equals()
* 해당 인스턴스를 매개변수로 전달받는 참조 변수와 비교화여 그 결과를 반환한다
* 이때 참조 변수가 가리키는 값을 비교하므로, 서로 다른 두 객체는 언제나 false를 반환한다
```java
public class EqualsSample{
    public static void main(String[] args) {
        String sample = "A";
        System.out.println(sample.eqauls("A"));
    }
}
```

<br>

### clone()
* 해당 인스턴스를 복제하여 새로운 인스턴스를 생성해 반환한다
* 하지만 Object 클래스의 clone() 메소드는 단지 필드의 값만 복사한다
* 필드의 갓이나 배열이나 인스턴스면 제대로 보게할 수 없다
* 이때는 오버라이딩이 필요하다

<br>

### 모든 Object 메소드
* protected Object clone()
* boolean equals(Object obj)
* protected void finalize()
* Class<T> getClass()
* int hashCode()
* void notify()
* void notifyAll()
* String toString()
* void wait()
* void wait(long timeOut)
* void wait(long timeOut, int nanos);