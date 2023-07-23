# Kotlin

![img.png](../🔲%20Image%20🔲/Kotlin/Kotlin-Logo.png)

* IntelliJ IDEA 개발사에서 공개한 오픈 소스 프로그래밍 언어
* JVM 기반의 언어이며, Java 와 유사하지만 더 간결한 문법과 다양한 기능을 추가하였다
* Java 와 상호 운용이 100% 지원된다
* JVM 바이트 코드가 기본이지만, Kotlin/Native 컴파일러를 사용하여 기계어로 컴파일할 수 있다
* 안드로이드, 스프링 프레임워크, Tomcat, JavaScript, Java EE, HTML5, iOS, 라즈베리 파이 등을 개발할 때 사용할 수 있다

<br>

# Kotlin : 문법

> 코틀린의 코드는 객체지향을 원칙으로 하며, 자바와 100% 연계되는 문법을 사용하고 있다

* .kt | .kts 형식의 저장 형식을 가진다
* 자바와 굉장히 비슷한 문법 구조를 가지고 있다

## 문법 : 메인 메소드

```kotlin
fun main() {
    // TODO
}
```

* 자바와 달리 메인 메소드의 길이가 상당히 짧다

```kotlin
fun main(args: Array<String>) {
    // TODO
}
```

* 1.3 버전 부터는 args 를 붙일 필요가 없게 되었으나 매개변수가 필요한 경우 args 나 array 를 붙이며 둘 다 사용법은 똑같다
* 독같이 작동하지만 JDK 환경인지 Native 로 장동하는지 등에 따라서 다르다

## 문법 : 타입

| 타입  | 예시                                                         |
|:---:|:-----------------------------------------------------------|
| 정수  | Long / ULong > Int / ULint > Short / UShort > Byte / UByte |
| 실수  | Double > Float                                             |
| 문자  | Char                                                       |
| 문자열 | String                                                     |

## 문법 : 변수 선언

```kotlin
fun main() {
    var a1: Int = 1
    var a2 = 1
    var b: String = "1"
    val c: Double = 3.141592

    println(a1) // OK
    println(b) // OK
    println(c) // OK

    a1 = a1++ // OK
    b = b + "2" // OK
    c = c++ // ERROR

}
```

* 타입을 적어줘도 적어주지 않아도 된다
* var 이 아니라 val 로 쓰게 된다면 c = c++ 처럼 값을 바꾸지 못한다
* 기본적으로 val 을 쓰는 것이 좋은 습관이다
* 하지만 자바에서 final 을 모든 변수에 쓰지 않듯이 대부분 var 로 사용한다
* 다만 val 을 블록 안에 쓰면 블록 범위 안에서만 동작하므로 run{} 같은 블록 안에 val 을 써서 val 의 동작 범위를 정해줄 수 있다

### 변수 선언 : null check

```kotlin
fun main() {
    var a: Int? = 1
    var b: Int? = 1

    println(a!! + b!!)

    var c: Int? = null // OK
    var d: Int = null // ERROR

}
```

* 기본적으로 `?` 를 타입 뒤에 붙이면 null 을 사용할 수 있다
* 하지만 둘 다 null 선언이 된 상태에서 값을 수정하거나 출력하려면 `null check` 에러가 뜨는 경우가 있다
* 이때는 `!!` 를 붙여주면 해결되는 경우도 있다

### 변수 선언 : 형변환 하기

```kotlin
fun main() {
    val a: Int = 1
    val b: String = "1"

    println(a + b) // ERROR
    println(a + b.toInt()) // OK

    val c: Int = 1
    val d: String = "1"

    println(c + d!!.toInt()) // OK

}
```

* String 과 Int 를 더하려면 오류가 나기 때문에 `.to타입()`을 붙여 타입을 변경할 수 있다
* null check 를 하는 경우 `d!!.toInt()`로 해주면 된다

## 문법 : 함수

```kotlin
fun main() {
    // TODO

    var Method: Method = Method()
    Method.Method1() // OK
    Method.Method2() // OK
}

class Method() {

    fun Method1() {
        println("Hello")
    }
    open fun Method2() {
        println("Hello")
    }
}
```

* 함수에 대한 기본 개념이 있다면 어떻게 사용하는지 문법만 배우면 된다

### 함수 : 확장 함수

> 이미 선언되어 있는 객체나 클래스 하위의 함수를 재정의 하거나 새로 정의할 수 있다

```kotlin
fun String.sayHello() {
    println("Hello, $this") // this는 객체 String을 가리킴
}

fun main() {
    "KimKiHyun".sayHello() // "Hello, KimKiHyun" 출력
}
```

### 함수 : infix 함수

> infix 키워드를 이용해서 . 과 () 를 쓰지 않아도 되는 함수를 만들 수 있다.

> #### 조건
> 1. 확장 함수 또는 클래스 함수여야 한다
> 2. 매개 변수가 1개여야 한다
> 3. 매개 변수는 기본 값이 없으면서 vararg 매개 변수가 없어야 한다

```kotlin
import java.net.*

class Human(var name: String, var age: Int, var location: String) {
    fun travel(location: String) {
        this.location = location
    }
    override fun toString(): String {
        return "${name}, ${age}세, ${location} 거주"
    }
    infix fun eat(food: String) = println("${this.name}님이 ${food}를 먹었습니다.")
}

fun main() {
    Human("홍길동", 30, "서울") eat "피자" // 홍길동님이 피자를 먹었습니다.
    Human("홍길동", 30, "서울") browse URL("https://namu.wiki/") // 홍길동님이 https://namu.wiki/를 검색했습니다.
}

infix fun Human.browse(url: URL) = println("${this.name}님이 ${url}를 검색했습니다.")
```

## 문법 : 스코프 함수

### 스코프 함수 : let

```kotlin
// let 이 있는 버전
class Human(var name: String, var age: Int, var location: String) {
    fun travel(location: String) {
        this.location = location
    }
    override fun toString(): String {
        return "${name}, ${age}세, ${location} 거주"
    }
}

fun main() {
    Human("홍길동", 30, "서울").let {
        println(it)
        it.travel("부산")
        println(it)
    }
}
```

```kotlin
// let 이 없는 버전
class Human(var name: String, var age: Int, var location: String) {
    fun travel(location: String) {
        this.location = location
    }
    override fun toString(): String {
        return "${name}, ${age}세, ${location} 거주"
    }
}

fun main() {
    val human = Human("홍길동", 30, "서울")
    println(human)
    human.travel("부산")
    println(human)
}
```

> #### let 을 사용하는 경우
> * null 이 가능한 오브젝트가 null 이 아닐 때 코드를 실행하게 할 때
> * 특정 변수를 제한적인 블록에서만 접근하게 만들 때

```kotlin
//null 이 가능한 오브젝트가 null 이 아닐 때 코드를 실행하게 할 때
val humans = ArrayList<Human>()


class Human(var name: String, var age: Int, var location: String) {
    fun travel(location: String) {
        this.location = location
    }
    override fun toString(): String {
        return "${name}, ${age}세, ${location} 거주"
    }
}

fun main() {

    Human("홍길동", 30, "서울").let {
        humans.add(it)
    }
    getHuman("홍길동")?.let {
        println(it.name) // 만약에 getHuman("홍길동")의 값이 null이라면 람다식을 실행되지 않는다.
    }
}

fun getHuman(name: String): Human? {
    return humans.firstOrNull {
        it.name == name
    }
}
```








































