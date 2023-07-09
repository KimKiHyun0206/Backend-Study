# AMQP
> 메시지 지향 미들 웨어를 위한 표준 응용 계층 프로토콜

# 등장 배경
> 서로 다른 시스템들 간의 최대한 효율적인 방법을 교환하깅 위한 AMMQ 프로토콜 등장
* 플랫폼 종속적인 재품들 사이에서 서로 다른 기종 간에 메시지를 교환이 필요해짐
* 교환을 위해서는 메시지 포멧 컨버전을 위한 메시지 브릿지를 이용 or 시스템 자체를 통일 시킨다
* 이는 불편하고 비효율적이다

# 특징
* 모든 브로커들은 똑같은 방식으로 동작할 것
* 모든 클라이언트들은 같은 방식으로 동작할 것
* 네트워크 상으로 전송되는 명령어들의 표준화
* 프로그래밍 언어 중립적

# AMQP : Routing Model
### Component 구성
* Exchange
* Queue
* Biding

![img.png](../🔲%20Image%20🔲/리펙토링 이전의 이미지/img6/AMQP-RoutingModel.png)

### Exchange
메시지 브로커에서 큐에세 메시지를 전달하는 역할

### Queue
> 메모리나 디스크에 메시지를 저장하고, 그것을 Consumer 에게 전달하는 역할
* 스스로가 관심 있는 메시지 타입을 지정한 Binding 을 통해 Exchange 에 말 그대로 묶인(bind)다

### Binding
Exchange 에 전달된 메시지가 어떤 큐에 저장되어야 하는지 정의

<br>

# Standard Exchange Type
### Direct Exchange
> 메시지의 라우팅 키를 큐에 1:1 로 매칭 시키는 방법

라운드 로빈 방식으로 여러 Consumer 간 task 분리

### Topic Exchange
> 와일드 카드를 이용해서 메시지를 큐에 매칭시키는 방법

* MultiCast 방식에 적합
* 라우팅 키는 `,`로 구분된 0개 이상의 단어의 집합으로 간주됨
  * 와일드카드 문자들을 이용해 특정 큐에 바인딩
  * `*` : 하나의 단어
  * `#` : 0개 이상의 단어
### Fanout Exchange
> 모든 메시지를 모든 큐로 라우팅하는 유형

1:N 방식으로 메시지를 브로드캐스트 하는 용도로 사용

### Header Exchange
> Key-Value 로 정의된 레더에 의해 라우팅 결정

큐를 바인딩할 때 x-match 라는 특별한 Argument 로 헤더을 어떤식으로 해석하고 바인딩할지 결정한다
* all : 모두 만족시켜야 한다 (and)
* any : 하나만 충족시키면 된다 (or)

<br>

# 사용 : AMQP
* RabbitMQ
* ActiveMQ
* ZeroMQ
* Apache Kafka

- - -
#### 🔗
* [AMQP 란?](https://velog.io/@gjrjr4545/AMQP)



