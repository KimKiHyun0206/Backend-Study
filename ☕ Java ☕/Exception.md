<div align="center">

![img.png](../🔲%20Image%20🔲/Java/Exception-예외의%20종류.png)

</div>

# Error

> #### 💡 시스템에 비정상적인 상황이 발생했을 경우에 발생한다

* 메모리 부족(OutOfMemoryError)
* 스택 오버플로우(StackOverflowError)
* 복구할 수 없다
* 개발자가 예측하기 쉽지 않으며 처리할 수 있는 방법이 없다

<br>

# Exception

> #### 💡 프로그램 실행 중에 개발자의 실수로 예기치 않은 상황이 발생했을 때

* 배열의 범위를 벗어남 = ArrayIndexOutOfBoundsException
* 값이 null 인 참조 변수를 참조 = NullPointerException
* 존재하지 않는 파일 이름을 입력 = FileNotFoundException
* 심각한 것은 아니기 때문에 복구할 수 없는 수준은 아니다
* Checked | Unchecked Exception 두 가지로 나눌 수 있다

## Exception : Checked Exception

> #### 💡 RuntimeException 의 하위 클래스가 아니면서 Exception 클래스의 하위 클래스들이다. 체크 예외의 특징은 반드시 에러 처리를 해야 하는 특징을 가지고 있다

* 값이 null 인 참조 변수를 참조 = NullPointerException
* 존재하지 않는 파일 이름을 입력 = FileNotFoundException

```java
public class TryCatchSample {
    
    public static void main(String[] args) {
        try {
            Scanner scanner = new Scanner(System.in);
            String str = scanner.nextLine();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

```java
public class ThrowSample {

    public void sample() throws Exception {
        throw new Exception();
    }
}
```

<br>

## Exception : Unchecked Exception
> #### 💡 RuntimeException 의 하위 클래스를 의미한다. 체크 예외와는 달리 에러 처리를 강제로 하지 않는다. 말 그대로 실행 중에 발생할 수 있는 예외를 의미한다

* 배열의 범위를 벗어난 = ArrayIndexOutOfBoundsException
* 값이 null 이 참조변수를 참조 = NullPointerException

> ### ❗ 왜 예외 처리를 하지 않을까?
> * 런타임 에러는 사용자의 입력에 의해서 발생하는 오류이다
> * 사용자의 입력을 모두 걸러낼 수는 겂다

<br>

|       비교        | Checked Exception                                                                                 | Unchecked Exception                                                                                                                   |
|:---------------:|:--------------------------------------------------------------------------------------------------|:--------------------------------------------------------------------------------------------------------------------------------------|
|      처리 여부      | 반드시 예외를 처리해야 함                                                                                    | 명시적인 처리를 강제하지 않음                                                                                                                      |
|      확인 시점      | 컴파일 단계                                                                                            | 실행 단계                                                                                                                                 |
| 예외 발생 시 트랜잭션 처리 | Rollback 하지 않는다                                                                                   | Rollback 함                                                                                                                            |
|      대표 예외      | Exception 의 상속을 받는 하위 클래스 중<br/>RuntimeException 을 제외한 모든 예외<br/>- IOException<br/>- SQLException | RuntimeException 하위 예외<br/>- NullPointerException<br/>- IllegalArgumentException<br/>- IndexOutOfBoundException<br/>- SystemException |





---
* [Exception](https://devlog-wjdrbs96.tistory.com/351)