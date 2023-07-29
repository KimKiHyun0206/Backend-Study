# 들어가기 앞서서

내가 가장 많이 사용하는 언어인 Java 의 버전 별 차이점을 알기 위해서 시작한다.

* Spring 은 3.x.x 부터 17 버전을 사용해야 하는 등의 차이점이 있기 때문이다

<br>

# JDK

> Java 는 처음에 발표되었을 때 JDK 란 이름으로 발표되었다

1. **JDK 1.0a** : 발표
2. **JDK 1.0a2** : 언어 자체가 정식으로 발표된 날
3. **JDL 1.0** : 발표 이전에 불렸던 이름은 Oak 였으며, 안정화 작업을 거친 1.0.2 버전에서 Java 로 이름이 바뀌었다
4. **JDK 1.1** : 이너 클래스, JavaBeans, RMI, 리플렉션, 유니코드 지원, 국제화(Internationalization) 등이 추가되었다

# J2SE

1. J2SE 1.2 : GIT, JIT, CORBA 등의 굵직한 기능이 추가되면서 2 부터 약칭을 J2SE(Java 2 Standard Edition) 으로 표기하기 시작했으며 이는 5까지 사용된다
    - strictfp, Swing GUI, JIT, Java Applet 을 구동하는 웹 브라우저 플러그인, CORBA, Collections 등이 추가되었다
    - 1999년에 업데이트를 통해 HotSpot JVM 이 처음 등장했다
2. J2SE 1.3 : HotSpot JVM, JNDI, JPDA, JavaSound 등이 추가되었다. RMI 가 CORBA 를 지원하도록 변경되었다
3. J2SE 1.4 : assert, 정규표현식, IPv6, Non-Blocking IO, XML API, JCE, JSSE, JAAS, Java Web Start 등이 추가되었다
4. J2SE 5 : Generics, Annotation, Auto Boxing/Unboxing, Enumeration, 가변 길이 파라미터, Static Import, 새로운 Concurrency API 등이
   추가되었다
    - Java 는 표준 입력 지원이 좋지 않았는데 이 버전부터 java.util.Scanner 가 추가되면서 표준 입력을 쉽게 사용할 수 있게 되었다
    - `1.` 표기가 없는 건 오타가 아니라 이 때부터 표기를 하지 않기 시작했기 때문이다

> ### 지원 종료에 관해
> * J2SE 는 모든 버전이 지원이 종료되었다

# Java SE

1. Java SE 6 : Scripting Language Support, JDBC 4.0, Java Compiler API, Pluggable Annotation 등이 추가되었다. 스크립팅 언어 지원과 함께
   Rhino JavaScript 엔진이 기본으로 탑재되었다
2. Java SE 7 : Dynamic Language 지원, switch 문에서 String 사용, try 문에서 자동 자원 관리, Diamond Operator <>, 이진수 리터럴, 숫자 리터럴에 _ 지원,
   새로운 Concurrency API, 새로운 File NIO 라이브러리, Elliptic Curve Cryptography, Java2D를 위한 XRender, Upstream, Java Deployment
   Ruleset 등이 추가되었다
3. Java SE 8 : Lambda Expression, Rhino 대신 Nashorn JavaScript 엔진 탑재, Annotation on Java Types, Unsigned Integer 계산,
   Repeating Annotation, 새로운 날짜와 시간 API, , Static Link JNI Library, Interface Default Method, PermGen 영역 삭제, Stream API
   등이 추가되었다
    - 32 비트를 지원하는 마지막 공식 Java 버전으로, 이후의 버전은 서드 파티를 통해서만 지원된다
    - 2023년 9월에 지원 종료 예정(유료 지원으로 연장 지원 가능성 있음)

### Java SE 9

> Project Jigsaw 기반으로 런타임이 모듈화된 것이 가장 큰 특징. 이에 따라 대부분의 콘솔 프로그램 개발에는 더 이상 AWT 나 Swing 같은 불필요한 라이브러리를 끌어쓸 필요도 없이, 최상위 모듈인
> Base 만 사용해도 된다. 더불어 특정 프로그램에 최적화된 최소 런타임을 제작할 수 있게 되면서 패키징 역시 간편해졌다

* Java 를 인터프리터 언어 셸처럼 사용할 수 있는 JShell 이 추가
* Java 바이트코드를 기계어로 미리 번역하는 선행 컴파일(Ahead-Of-Time Compilation) 실험 기능으로 추가
* Deprecated 표시에는 해당 버전과 제거 예정 여부를 표시할 수 있게 되었다
* 구조적 불변 컬렉션, 통합 로깅, HTTP/2, private 인터페이스 메소드, HTML5 Javadoc 등도 지원되며, 프로퍼티 파일에 UTF-8이 지원됨에 따라 더 이상 인코딩 문제로 삽질할 필요가
  없어졌다. 또한 Java Applet 기능은 지원이 종료된다
* Deprecated API 는 다음 버전인 Java SE 10부터 완전 삭제 예정이므로 해당 API 를 쓰는 프로그램은 더 이상 이후의 버전에서 컴파일조차 불가능하게 된다

> ### 버전 이름
> 이전의 버전은 Java SE8 이 1.8 이었다. 하지만 이번 Java SE 9 부터 9.0 으로 올림을 시켰다

* 이 버전부터 64 비트 버전만 출시되며, 32 비트는 더이상 공식적으로 나오지 않는다

### Java SE 10

* var 키워드를 이용한 지역 변수 타입 추론 기능 추가
* kotlin 과 동일한 기능이다
* 병렬 처리 가비지 컬렉션 추가
* 개별 스레드로 분리된 Stop-The-World
* 루트 CA 목록 추가
* JDK 레보지토리가 하나로 통합
* JVM 힙 영역을 시스템 메모리가 아닌 다른 종류의 메모리에도 할당할 수 있게 되었다
* 실험 기능으로 Java 기반의 JIT 컴파일러가 추가됨
* 이전 버전에서 Deprecated 처리된 API 는 Java SE 10 에서 모두 삭제되었다

### Java SE 11

> 이클립스 재단으로 넘어간 Java EE가 JDK 에서 삭제되고, JavaFX도 JDK 에서 분리되어 별도의 모듈로 제공된다

* Gloun 이라는 업체가 JavaFX를 유지보수 중
* 람다 파라미터에 대한 지역 변수 문법, 엡실론 가비지 컬렉터, HTTP 클라이언트 표준화 등의 기능이 추가
* **가장 커다란 변화는 바로 라이선스 부분**. Java SE 11부터 Oracle JDK 의 독점 기능이 오픈 소스 버전인 OpenJDK 에 이식된다
    * Oracle JDK 와 OpenJDK 가 완전히 동일해진다는 뜻

### Java SE 12

> Switch 문의 확장

```java
public class SwitchSample {
    public static void main(String[] args) {
        switch (day) {
            case MONDAY, FRIDAY, SUNDAY -> System.out.println(6);
            case TUESDAY -> System.out.println(7);
            case THURSDAY, SATURDAY -> System.out.println(8);
            case WEDNESDAY -> System.out.println(9);
        }
    }
}
```

* 이 외에 가비지 컬렉터 개선
* 마이크로 벤치마크 툴 추가
* 성능 개선의 변경점이 있다

### Java SE 13

> Java 12에서의 스위치 개선을 이어 yield 라는 예약어가 추가되었다.

```java
public class Sample {
    public static void main(String[] args) {
        var a = switch (day) {
            case MONDAY, FRIDAY, SUNDAY:
                yield 6;
            case TUESDAY:
                yield 7;
            case THURSDAY, SATURDAY:
                yield 8;
            case WEDNESDAY:
                yield 9;
        };
    }
}
```

### Java SE 14

> 프리뷰 기능으로 instanceof 의 패턴 매칭과 record 라는 데이터 오브젝트 선언이 추가되었다. 그 외에 인큐베이터라는 패키징 툴(OS에 맞춘 실행파일 생성기능) 추가 등이 있다

```java
public class Instanceof {
    public static void main(String[] args) {
        if (!(obj instanceof String s)) {
    ..s.contains(..) ..
        } else {
    ..s.contains(..) ..
        }
    }
}
```

```java
record Point(int x, int y) {
}
```

### Java SE 15

* EdDSA 암호화 알고리즘 추가
* 패턴 매칭 (2차 미리보기, 상단 Java 14 참조)
* 스케일링 가능한 낮은 지연의 가비지 컬렉터 추가(ZGC)
* Solaris 및 SPARC 플랫폼 지원 제거
* 외부 메모리 접근 API (인큐베이팅)
* 레코드 (2차 미리보기, 상단 Java 14 참조)
* 클래스 봉인 (미리보기)
* 상속 가능한 클래스를 지정할 수 있는 봉인 클래스가 제공된다.
* 상속 가능한 대상은 상위 클래스 또는 인터페이스 패키지 내에 속해 있어야 한다.
* 다중 텍스트 블록 : 여러 줄 문자열을 편하게 작성할 수 있다

```java
public sealed class Shape
        permits Circle, Square, Rectangle {
}
```

```java
public class Sample {
    String html = """
            <html>
                <body>
                    <p>Hello, world</p>
                </body>
            </html>
            """;
}
```

### Java SE 16
* JEP 338: Vector API
* 자바에서도 자동 병렬 프로세싱을 지원하는 자동 벡터 API 가 추가될 예정이다.
* JEP 347: 자바 네이티브(JNI 등) 개발 시 C++14 규격을 지원하기 시작했다.
* OpenJDK 의 버전 관리가 Mercurial 이었으나, 이제 Git 으로 바뀐다.
* 이제 OpenJDK 소스를 GitHub 에서 볼 수 있다.
* 자바 11부터 시작했으며 15부터 정식으로 도입한 ZGC 기능이 향상되었다.
* 유닉스 도메인 소켓이 지원된다.
* Alpine Linux 리눅스 지원 추가
* JEP 387: 자바 8부터 제거된 악명높은 PermGen 대신 Metaspace 방식을 지원하기 시작한다.
* ARM 64비트 윈도우 운영체제가 지원된다. MacOS의 경우 실리콘 맥 지원이 시험 빌드에서 네이티브 지원을 시작했다.
* JEP 389: JNI 를 대신할 외부 링크 방식의 인터페이스를 인큐베이팅을 통해 시작한다.
* JEP 390: 값 유형의 클래스를 동기화에 사용 시 경고 메시지가 개선되었다.

```java
public class JEP390 {

    public static void main(String[] args) {

        Double d = 20.0;
        synchronized (d) {} // 컴파일 시 해당 줄 내용과 함께한 경고 메시지가 친절히 표시된다.
    }
}
```
* JEP 343: jpackage 명령어를 통해 각 운영체제별 자바 프로그램을 설치 패키지(pkg, deb, msi 등)로 생성하는 기능이 정식으로 추가되어, 자바 프로그램을 손쉽게 배포하는 기능이 추가된다.
  
* 자바 15의 외부메모리 접근 인큐베이팅 2차
* 자바 14의 패턴 매칭이 정식 기능으로 추가
* 자바 14에 추가된 Record 형식 정식 지원
* 자바 9부터 추가되어 자바 내부 API 접근에 대한 경고 무시 (--illegal-access) 기능이 강화되어 내부 API 접근 시도 시 기본적으로 오류와 함께 자바 프로그램이 종료될 수 있도록 강화
* 자바 15에 추가된 봉인 클래스의 2차 미리보기



### Java SE 17
* 패턴 매칭 : 여전이 Preview 단계
* 외부 함수/메모리 API 및 신규 벡터 API 는 인큐베이팅 단계
* JEP 398: 애플릿이 완전히 제거될 예정으로 Deprecated 처리.
* JEP 391: 애플 M1 및 이후(MacOS/AArch64) 프로세서 탑재 컴퓨터 제품군에 대한 정식 지원
* JEP 382: macOS 그래픽 렌더링 베이스를 OpenGL 에서 Metal 로 교체
* JEP 356: 의사난수 생성기를 통해 예측하기 어려운 난수를 생성하는 API 정식 추가
* JEP 415: 컨텍스트 기반의 역직렬화 필터링
* JEP 409: 봉인 클래스가 정식 추가되었다.

### Java SE 18
* JEP 400: 자바 API 의 기본 Charset 이 UTF-8으로 지정되었다.
* JEP 408: 정적 파일을 서빙하는 기능만 있는 심플한 웹 서버 제공 (커맨드라인 툴)
* JEP 413: Java API Doc 에 @snippet 태그 추가
* JEP 416: 리플렉션 기능 리팩터링 (메소드 핸들을 이용해 다시 구현)
* JEP 417: 벡터 API (세 번째 인큐베이터 단계)
* JEP 418: Internet-Address Resolution SPI
* JEP 419: 외부 함수 & 메모리 API (두 번째 인큐베이터 단계)
* JEP 420: switch 문 패턴매칭 (두 번째 프리뷰)
* JEP 421: try 문의 finally 기능 deprecate.
* 아직 제거되진 않았지만 사용을 권장하지 않는다. try-with-resources 를 이용하자.

### Java SE 19
* JEP 405: 레코드 패턴매칭 (프리뷰)
* JEP 422: Linux/RISC-V Port 
* JEP 424: 외부 함수 & 메모리 API (프리뷰)
* JEP 425: 가상 쓰레드 (프리뷰)
* JEP 426: 벡터 API (네 번째 인큐베이터 단계)
* JEP 427: switch 문의 패턴 매칭 (세 번째 프리뷰)
* JEP 428: 멀티쓰레드 프로그래밍을 단순화하는 Structured Concurrency API (인큐베이터 단계)


### Java SE 20
* JEP 429 : Scoped Values (인큐베이터 단계)
* JEP 432 : 레코드 패턴 매칭 (두 번째 프리뷰)
* JEP 433 : switch 문 패턴 매칭 (네 번째 프리뷰)
* JEP 434 : 외부 함수 & 메모리 API (두 번째 프리뷰)
* JEP 436 : 가상 쓰레드 (두 번째 프리뷰)
* JEP 437 : Structured Concurrency (두 번째 인큐베이터 단계)

### Java SE 21
출시 예정

<br>

# 마치며
* 다양한 자바 버전이 있다는 것을 알게 되었다
* 한 번으로 이걸 다 암기하는 것은 안 되니 나중에 복습을 꼭 해야겠다
* 내가 11 버전과 17 버전만 사용했는데 앞으로는 다양한 버전을 사용해 보면서 추가된 기능을 사용해 보아야겠다