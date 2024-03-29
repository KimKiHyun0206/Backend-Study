# Kafka(Apache Kafka)

> 파이프라인, 스트리밍 분석, 데이터 통합 및 미션 크리티컬 애플리케이션을 위해 설계된 **고성능 분산 이벤트 스트리밍 플랫폼**

* Pub-Sub 모델의 Message Queue 형태로 동작하며 분산 환경에 특화되어있다

# Kafka 의 탄생

> Kafka 개발 전 Linked0In 데이터 처리 시스템

![img.png](../🔲%20Image%20🔲/리펙토링 이전의 이미지/img6/Kafka%20개발%20전%20링크드인의%20데이터%20처리%20시스템.png)

## 기존 데이터 시스템의 문제점

* 각 애플리케이션과 DB가 End-to-End 로 연결되어 있고 각 파이프라인이 파편화 되어있음
* 요구 사항이 늘어남에 따라 데이터 시스템 복잡도가 높아지면서 다음과 같은 문제가 발생하게 됨

### 1. 시스템 복잡도 증가 (Complexity)

* 통합된 전송 영역이 없어 데이터 흐름을 파악하기 어렵고, 시스템 관리가 어려움
* 특정 부분에서 장애 발생 시 조치 시간 증가
    * 연결되어 있는 애플리케이션 모두 확인해야 하기 때문이다
* Hardware 교체 | Software 업그레이드 시 관리 포인트가 늘어나고 작업 시간 증가
    * 연결된 애플리케이션에 Side Effect 가 없는지 확인해야 한다

### 2. 데이터 파이프라인 관리 어려움

* 각 애플리케이션과 데이터 시스템 간의 별도 파이프라인이 존재하고, 파이프라인마다 데이터 포멧과 처리 방식이 다름
* 새로운 파이프라인 확장이 어려워지면서 확장성 및 유연성이 떨어짐
* 또한 데이터 불일치 가능성이 있어 신뢰도 감소

> **이 문제를 해결하기 위해 모든 이벤트와 데이터의 흐름을 중앙에서 관리하는 카프카를 개발하게 되었다**

<br>

## 카프카 적용 후

> Kafka 를 적용한 후 Linked-In 데이터 처리 시스템

![img.png](../🔲%20Image%20🔲/리펙토링 이전의 이미지/img6/Kafka%20적용%20후.png)

* 모든 이벤트 | 데이터 흐름을 중앙에서 관리할 수 있게 됨
* 새로운 서비스 | 시스템이 추가되도 카프카가 제공하는 표준 포멧으로 연결하면 되므로 확장성과 신뢰성이 증가
* 개발자는 각 서비스 간의 연결이 아닌, 서비스들의 비즈니스 로직에 집중 가능

<br>

# Kafka 의 동작 방식 및 특징

> Kafka 는 Pub-Sub 모델의 Message Queue 형태로 동작한다

* Message Queue 의 개념 이해 필요
* Broker 개념 이해 필요

## Message Broker

> Publisher 가 생산한 메시지를 큐에 저장하고 저장된 데이터를 Consumer 가 가져갈 수 있도록 중간 다리를 해주는 브로커

* 보통 서로 다른 시스템(혹은 소프트웨어) 사이에서 데이터를 비동기 형태로 처리하기 위해 사용한다
* 이러한 구조를 보통 Pub-Sub 구조라고 하며 대표적으로 Redis, RabbitMQ 가 있다
* 이와 같은 메시지 브로커들은 Consumer 가 큐에서 데이터를 가져가게 되면 즉시 혹은 짧은 시간 내에 큐에서 데이터가 삭제된다

## Event Broker

> 기본적으로 메시지 브로커의 큐 기능들을 가지고 있어 메시지 브로커의 역할도 할 수 있다

* 그래도 차이점은 있다
* 이벤트 브로커는 Publisher 가 생성한 이벤트를 처리한 후에 바로 삭제하지 않고 저장하여, 이벤트 시점이 저장되어 있어서 Consumer 가 특정 시점부터 이벤트를 다시 Consume 할 수 있는 장점이
  있따
    * 이것은 장애가 일어난 시점부터 그 이후의 이벤트를 다시 처리할 수 있게 해준다
* 대용량 처리에 있어서 메시지 브로커보다 더 많은 양의 데이터를 처리할 수 있는 능력이 있다
* 이벤트 브로커에는 Kafka, AWS, kinesis 같은 서비스가 있다

<br>

## 문제 : 일반적인 형태의 네트워크 통신 (Kafka 가 아닌 것)

![img.png](../🔲%20Image%20🔲/리펙토링 이전의 이미지/img6/Kafka-일반적인%20형태의%20네트워크%20통신.png)
> 각 개체가 직접 연결하며 통신하게 된다

#### 장점

* 전송 속도가 빠르다
* 전송 결과를 신속하게 알 수 있다

#### 단점

* 특정 개체에 장애가 발생한 경우 메시지를 보내는 쪽에서 대기 처리등을 개별적으로 해주지 않으면 장애 전파 가능
* 참여하는 개체가 많아질수록 각 개체를 연결해주어야 한다
    * 시스템이 커질수록 확장성이 좋지 않아진다

## 해결 : Pub-Sub 모델

> 비동기 메시징 전송 방식

![img.png](../🔲%20Image%20🔲/리펙토링 이전의 이미지/img6/Kafka-Pub-Sub%20모델.png)

* 발신자의 메시지에는 수신자가 정해져 있지 않은 상태로 Publish 한다
* 이를 Subscribe(구독) 한 수신자만 정해진 메시지(Topic)을 받을 수 있다
* 이처럼 수신자는 발신자의 정보가 없어도 원하는 메시지만 수신할 수 있다
* 이런 구조 덕분에 높은 확장성을 보여준다

<br>

# 언제 Kafka 를 사용해야 할까?

* 대용량 데이터 처리
* 실시간
* 고성능
* 고가용성
* 저장된 이벤트를 기반으로 로그를 추적하고 재처리

<br>

# Kafka 의 구성 요소

![img.png](../🔲%20Image%20🔲/리펙토링 이전의 이미지/img6//Kafka-구성%20요소.png)

### Topic

* 각각의 메시지를 목적에 맞게 구분할 때 사용한다
* 메시지를 전송하거나 소비할 때 Topic 을 반드시 입력한다
* Consumer 는 자신이 담당하는 Topic 의 메시지를 처리한다
* 한 개의 토픽은 한 개 이상의 파티션으로 구성된다

### Partition

* 분산 처리를 위해 사용된다
* Topic 생성 시 Partition 개수를 지정할 수 있다 (파티션 개수 추가 가능, 감소 불가)
* 파티션이 1 개라면 모든 메시지에 대해 순서가 보장된다
* 파티션이 내부에서 각 메시지는 Offset(고유번호)로 구분된다
* 파티션이 여러 개라면 Kafka 클러스터가 라운드 로빈 방식으로 분배해서 분산 처리되기 때문에 순서 보장 X
* 파티션이 많을 수록 처리량이 좋지만 장애 복구 시간이 늘어난다

### Offset

* Consumer 에서 메시지를 어디까지 읽었는지 저장하는 값
* Consumer Group 의 Consumer 들은 각각의 파티션에 자신이 가져간 메시지의 위치 정보(Offset)을 기록
* Consumer 장애 발생 후 다시 살아나도, 전에 마지막으로 읽었던 위치에서부터 다시 읽을 수 있다

### Producer

* 메시지를 만들어서 Kafka Cluster 에 전송한다
* 메시지 전송 시 Batch 처리가 가능하다
* Key 값을 지정하여 특정 파티션으로 전송 가능
* 전송 Acks 값을 설정하여 효율성을 높일 수 있다
    * Acks = 0 : 매우 빠르게 전송, 파티션 리더가 받았는지 알 수 없다
    * Acks = 1 : 파티션 리더가 받았는지 확인, 기본 값
    * Acks = ALL : 파티션 리더 뿐만 아니라 팔로워까지 메시지를 받았는지 확인

### Consumer

* Kafka Cluster 에서 메시지를 읽어서 처리한다
* 메시지를 Batch 처리할 수 있다
* 한 개의 Consumer 는 여러 개의 Topic 을 처리할 수 있다
* 메시지를 소비하여도 메시지를 삭제하지 않는다
    * Kafka Delete Policy 에 의해 삭제된다
    * 한 번 저장된 메시지를 여러 번 소비 가능하다
    * Consumer 는 Consumer Group 에 속한다
    * 한 개의 Partition 은 같은 Consumer Group 의 여러 개의 Consumer 에서 연결할 수 있다

### Broker

* 실행된 Kafka Server 를 말한다
* Producer 와 Consumer 는 별도의 애플리케이션으로 구성되는 반ㅁ녀, 브로커는 Kafka 자체이다
* Broker(각 서버)는 Kafka Cluster 내부에 존재한다
* 서버 내부에 메시지를 저장하고 관리하는 역할을 수행한다

### Zookeeper

* 분산 애플리케이션 관리를 위한 코디네이션 시스템
* 분산 메시지 큐의 메타 정보를 중앙에서 관리하는 역할

<br>

# 주요 설계 특징

## 왜 하나의 Topic 을 여러 개의 Partition 으로 분산시키는가

![img.png](../🔲%20Image%20🔲/리펙토링 이전의 이미지/img6/Kafka-Topic%20Partition%20분산.png)

* 병렬로 처리하기 위해 분산 저장한다
* Kafka 의 Topic 에 메시지가 쓰여지는 것도 어느 정도 시간이 소비된다
    * 몇 천의 메시지가 동시에 Kafka 에 Write 되면 병목 현상이 발생할 수 있다
* 타라서 파티션을 여러 개 두어 분산 저장함으로써 Write 동작을 병렬로 처리할 수 있다
* 하지만 한 번 늘린 파티션은 절대 줄일 수 없디 때문에 운영 중에 파티션을 늘려야 하는 건 충분히 검토된 후에 실행되어야 한다
    * 최소한의 파티션으로 운영하고 사용량에 따라 늘리는 것이 좋다
* 파티션을 늘릴 때 메시지는 Round-Robin 방식으로 쓰여진다
    * 하나의 파티션 내에서는 메시지 순서가 보장
    * 파티션이 여러 개인 경우 순서 보장 X

<br>

## Consumer Group 은 왜 존재할까

![img.png](../🔲%20Image%20🔲/리펙토링 이전의 이미지/img6/Kafka-Consumer%20Group.png)

* Consumer 의 묶음을 Consumer Group 이라고 한다
* Consumer Group 은 하나가 Topic 에 대한 책임을 가지고 있다
* 어떤 Consumer 가 Down 된다면, 파티션 재조정(Rebalancing)을 통해 다른 Consumer 가 해당 파티션의 Sub 을 맡아서 한다
* Offset 정보를 그룹 간에 공유하고 있기 때문에 Down 되기 전에 마지막으로 읽었던 메시지 위치부터 시작한다

<br>
<br>
<br>

---
#### 🔗
* [Kafka 란?](https://velog.io/@holicme7/Apache-Kafka-%EC%B9%B4%ED%94%84%EC%B9%B4%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%B8%EA%B0%80)