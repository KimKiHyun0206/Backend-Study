# Java 메모리 구조

<br>

# JVM

## JVM : 동작 방식

![img.png](../🔲%20Image%20🔲/Java/Java%20메모리%20구조-JVM의%20동작%20방식.png)

1. 자바로 개발된 프로그램을 실행하면 JVM 은 OS 로부터 메모리를 할당합니다.
2. 자바 컴파일러(javac)가 자바 소스코드(.java)를 자바 바이트코드(.class)로 컴파일합니다.
3. Class Loader 를 통해 JVM Runtime Data Area 로 로딩합니다.
4. Runtime Data Area 에 로딩 된 `.class` 들은 Execution Engine 을 통해 해석합니다.
5. 해석된 바이트 코드는 Runtime Data Area 의 각 영역에 배치되어 수행하며 이 과정에서 Execution Engine 에 의해 GC의 작동과 스레드 동기화가 이루어집니다.

## JVM : 구조

### 클래스 로더, Class Loader

> 💡 **자바는 동적으로 클래스를 읽어오므로, 프로그램이 실행 중인 런타임에서야 모든 코드가 자바 가상 머신과 연결된다. 이렇게 동적으로 클래스를 로딩해주는 역할을 하는 것이 클래스 로더이다**

![img.png](../🔲%20Image%20🔲/Java/Java%20메모리%20구조-클래스%20로더.png)

* 자바에서 소스를 작성하면 `.java` 파일이 생성되고 `.java` 소스를 컴파일러가 컴파일 하면 `.class` 파일이 생성된다
* 클래스 로더는 `.class` 파일을 묶어서 JVM 이 운영체제로부터 할당받은 메모리 영역인 Runtime Data Area 로 적재한다

### 실행 엔진, Execution Engine

> 💡 **.class 파일(바이트 코드)들은 Runtime Data Area 의 Method Area 에 배치되는데, 배치된 이후에 JVM 은 Method Area 의 바이트 코드를 실행 엔진에 제공하여 정의된 내용대로 바이트 코드를 실행 시킨다**

* 이때 로드된 바이트 코드를 실행하는 런타임 모듈이 실행 엔진이다
* 실행 엔진은 바이트 코드를 명령어 단위로 읽어서 실행한다

### 가비지 컬렉터, Garbage Collector

> 💡 **JVM 은 가비지 컬렉터를 이용하여 더는 사용하지 않는 메모리를 자동으로 회수해 준다**

![img.png](../🔲%20Image%20🔲/Java/Java%20메모리%20구조-가비지%20컬렉터.png)

* 개발자가 따로 메모리를 관리하지 않아도 된다 = 더 쉬운 프로그래밍
* Heap 메모리 영역에 생성(적재)된 객체들 중에 참조되지 않은 객체들은 탐색 후 제거하는 역할을 하며 해당 역할을 하는 시간은 정확히 언제인지 알 수 없다
* GC 역할을 수행하는 스레드를 제외한 나머지 모든 스레드들은 일시정지 상태가 된다

<br>
<br>

# 런타임 데이터 영역, Runtime Data Area

> 💡 **런타임 데이터 영역은 JVM 의 메모리 영역으로 자바 애플리케이션을 실행할 때 사용되는 데이터들을 적재하는 영역이다**

![img.png](../🔲%20Image%20🔲/Java/Java%20메모리%20구조-런타임%20데이터%20영역.png)

#### 모든 스레드가 공유해서 사용(GC의 대상)

* 힙 영역, Head Area
* 메소드 영역, Method Area

#### 스레드(Thread)마다 하나씩 생성

* 스택 영역, Stack Area
* PC 레지스터, PC Register
* 네이티브 메소드 스택, Native Method Stack

<br>

## 힙 영역, Heap Area

> 💡 **new 키워드로 생성된 객체와 배열이 생성되는 영역이며 주기적으로 GC 가 제거한다**

![img.png](../🔲%20Image%20🔲/Java/Java%20메모리%20구조-힙%20영역.png)

Heap Area 는 효율적인 GC 를 위해 세 가지 영역으로 나뉜다

* 모든 스레드에서 정보가 공유된다
* Reference Type 의 데이터가 저장되는 공간
* Heap 의 데이터는 GC 가 처리하지 않는 한 소멸되지 않는다

### Young Generation

* 자바 객체가 생성되자마자 저장된다
* 생긴지 얼마 안 되는 객체가 저장되는 공간
* 영역이 가득 차게 되면 참조 정도에 따라 Old 영역으로 이동하거나 회구된다

### Old

* 할당된 메모리가 허용치를 넘게 되면, Old 영역에 있는 모든 객체들을 검사하여 참조되지 않는 객체들을 한꺼번에 삭제하는 GC가 실행
* 시간이 오래 걸리는 작업이고 이때 GC 를 실행하는 스레드를 제외한 모든 스레드는 작업을 멈춘다

### Eden

* 객체가 생성되면 할당되는 영역
* 참조 횟수에 따라 Survivor 영역으로 이동되거나 회수된다

### Survivor

* Eden 영역에 어느 정도 데이터가 쌓이면 참조 정도에 따라 이동된다

> #### ❗ Minor GC
> Young Generation 과 Tenured Generation 에서의 GC

> #### ❗ Stop-the-World
> GC 를 실행하는 스레드를 제외한 모든 스레드는 작업을 멈춘다

> #### ❗ Major GC
> Old 영역의 메모리를 회수하는 GC

## 메소드 영역, Method Area

> 💡 **하위 목록들이 생성되는 영역**

* 각종 필드 정보
    * 클래스 멤버 변수 이름
    * 데이터 타입
    * 접근 제어자 정보
* 데이터 Type 정보
* Runtime Constant Pool : 상수 정보가 저장되는 공간
* static 변수
* final class 등

<br>

* JVM 이 실행되면서 생기는 공간
* 모든 스레드에서 정보가 공유된다

## 스택 영역, Stack Area

> 💡 **지역 변수, 파라미터, 리턴 값, 연산에 사용되는 임시 값 등이 생성되는 영역**

* 중괄호를 빠져나오면 사라지는 영역이라고 생각하면 편하다
* Last In First Out
* 만약 지역 변수이지만 Reference Type 인 경우 Heap 에 저장된 데이터의 주소값을 Stack 에 저장해서 사용한다
* 스레드마다 하나씩 존재한다

## PC 레지스터, PC Register

> 💡 **Thread 가 생성될 때마다 생성되는 영역**

* 프로그램 카운터
* 현재 스레드가 실행되는 부분의 주소와 명령을 저장하는 영역

## 네이티브 메소드 스택

1. 자바 이외의 언어(C, C++, 어셈블리 등)로 작성된 네이티브 코드를 실행할 때 사용되는 메모리 영역으로 일반적인 C 스택을 사용합니다.
2. 보통 C/C++ 등의 코드를 수행하기 위한 스택을 말하며 (JNI) 자바 컴파일러에 의해 변환된 자바 바이트 코드를 읽고 해석하는 역할을 하는 것이 자바 인터프리터(interpreter)입니다.

---

* [JVM](https://coding-factory.tistory.com/828)
* [Rumtime Data Area](https://velog.io/@shin_stealer/%EC%9E%90%EB%B0%94%EC%9D%98-%EB%A9%94%EB%AA%A8%EB%A6%AC-%EA%B5%AC%EC%A1%B0)


















