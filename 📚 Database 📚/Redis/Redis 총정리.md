# Redis

* Key-Value(비정형 데이터) 구조로 저장
* 오픈 소스 기반
* 비관계형 데이터베이스(DBMS)
* DB, Cache, Message Broker 로 사용
* In-Memory 데이터 구조를 가진 저장소

## Redis 의 특징

* `No Query` : Key-Value 구조이기 때문에 쿼리 사용 X
* `빠르다` : 데이터를 디스크에 쓰는 구조 X | 메모리에서 데이터를 처리
* `다양한 자료구조` : String, Lists, Sets, Sorted Sets, Hashes
* `Single Thread` : 한 번에 한 명령어만 처리할 수 있다
* `Multi Tread` : IO Socket Read/Write 동작 시 멀티 스레드로 동작

> ### 사용 시 주의점
> * 서버에 장애 발생할 경우 대비책 필요
> * 메모리 관리 중요
> * 싱글 스레드 특성 때문에 오래 걸리는 명령은 피해야 한다

## Cache Server

> **서버의 부하를 줄이기 위해 사용한다. 캐시 서버의 역할은 한 번 읽은 데이터를 임의의 공간에 저장하여 다음에 읽을 때 결과값을 빠르게 받을 수 있도록 하는 공간이다**

* 데이터베이스는 물리 디스크에 직접 저장(쓰기)한다
* 이는 문제가 발생하여 다운 되더라도 데이터가 손실되지 않는 장점이 있다
* But, 매번 디스크에 접근 = 사용자가 많아지면 부하가 많아진다(느려진다)
* 이 부하를 줄이기 위해 사용되는 것이다

> 서버의 부하를 줄이고 서비스의 속도를 유지할 수 있다

<br>
<br>

# Redis Topology

> **Redis Architecture 의 종류**

* Master-Slave 형태로 데이터를 복제해서 운영할 수 있다
* Master-Slave 간 복제는 Non-Blocking 상태로 이루어진다

> ### Redis Topology 와 Redis Cluster
> Cluster 는 한 개의 Master 에 여러 개의 Slave 를 둘 수 있다는 것을 전제로 만들어진 것이다.
> 이는 Cluster 가 구현되는데 Topology 가 필요함을 나타낸다.

* 노드 간의 파티셔닝이 가능하게 해준다
* 데이터를 슬롯으로 나누고 클러스터 노드에 분산하여 저장할 수 있다

> ### Redis Topology 와 Redis Sentinal
> Sentinal 은 Master 와 Replica(Slave)로 구성된다. 이 부분에서 Redis Topology 의 Master-Slave 구조가 필요하다

* 장애를 자동으로 감지하고 현재 Master 가 실패할 경우 Slave 하나를 새로운 Master 로 승격시킴으로써 Failover 을 수행한다

<br>
<br>

# Redis Replication

> **Redis 의 데이터를 거의 실시간으로 다른 레디스 노드에 복사하는 작업**

* 서비스를 제공하던 Master 가 다운되더라도, 데이터를 받은 Slave 가 서비스를 계속할 수 있다

## 복제 기능

* **비동기(Asynchronous) 복제**를 한다
* **Master 는 여러 개의 Replica** 를 둘 수 있다
* **Replica 는 또 Replica 를 둘 수 있다**
* **RDB 파일** : Master 가 Replica 로 데이터를 보낼 때 사용하는 파일
* 복제 작업은 **자식 프로세스**가 처리한다
* **데이터 유실 가능성** : Master 의 부하를 줄이기 위해 ,AOF 쓰기나 RDB 파일을 생성해서 Replica 에서 수행하는 방법

> ### 동기화 종류
> * 전체 동기화
> * 부분 동기화

### 만약 복제 기능이 없다면?

> **Redis Instance(Master or Slave)가 모종의 이유로 다운되었을 때, 관리자가 이를 인지하고 Redis 를 문제 없이 다시 시작했다면 산관 없다. 하지만 보통은 그렇지 않다.**

* AOF 기능을 사용하고 데이터가 많은 경우 인스턴스 재시작이 오래 걸린다
* 하드웨어 문제로 다운 = 다시 시작에 상당한 시간 소요
* 데이터 복구 불확실

## Replica

* 디폴트로 읽기 전용이다(데이터를 저장할 수 없다)
* Replica 에 데이터를 입력 해도 Master 와 `Resync` 되면 입력된 데이터는 사라진다

<br>
<br>

# Redis Sharding

> 데이터 저장 공간을 확보하는 방법

* Scale-Up 만으로는 데이터 저장 공간을 충분히 확보하지 못할 수 있다
* Replication 은 모든 데이터를 복제해야 하기 때문에 단일 서버에서 저장 가능한 양을 초과하면 복제할 수 없다

## 개념 : Partitioning

> **DB 의 관리 용이성 및 읽기 최적화를 위해 논리적인 테이블의 물리 구조를 여러 개 파티션(Partition)으로 분할하여 분산 저장하는 기법**

* DB 에서 테이블의 크기가 계속 커진다면, 해당 테이블의 조회 성능이 계속해서 낮아진다
    * 따라서 인덱싱 같은 작업을 하는 것이다
    * 하지만 작업들은 쉽지 않으며 작업을 하여도 조회 성능이 떨어지게 된다

> 이런 경우에 사용자에게 논리적으로 보여지는 테이블은 하나지만 물리적으로는 여러 파티션에 데이터를 나누어 저장하면 조회 성능 향상 및 관리가 용이해진다

### 이점

> **대용향의 논리적 구조를 여러 물리적인 파티션으로 분할하여 조회 및 관리 용이성을 위해 사용한다**

* 데이터의 범위를 기준으로 파티셔닝을 한다면 범위에 따라 조회하는 곳을 찾아가면 된다
* 만약 삭제할 경우 그 범위만 Drop 하면 된다

## Partition 의 종류

> 크게 수직적(Vertical)과 수평적(Horizontal)이 있다

### Vertical Partitioning

> **특정 데이터를 기준으로 데이터를 다른 파티션에 저장하는 방법**

* 위에서 예시를 들었던 것
* 날짜 : 월 , 년도
* 사람 : 남성, 여성

### Horizontal Partitioning

> **특정 컬럼을 기준으로 데이터를 분할**

* 1~10열이 있다면 1~5 / 6~10 로 나눈다
* 정규화와 비슷하다고 생각하면 된다
* `장점` : 한쪽 세그먼트에서 발생하는 DML 이 다른 쪽에 영향을 끼치지 않는다
* `단점` :레코드 전체 데이터를 읽어야할 경우에는 비효율적이다
    * 데이터가 물리적으로 나누어져 있기 때문이다

### NoSQL 의 Sharding

> **수평적 파티셔닝의 한 종류**

> 용량을 고려하여 데이터 크기를 분할할 때, 수직적 파티션 보다는 수평적 파티션이 분배에 용이하다

* 수평적 파티셔닝과 비교하여 다른 점은 파티셔닝은 단일 DBMS 내에서 데이터 분할 정책이고, 샤딩은 여러 데이터베이스 서버로부터 데이터를 분할하는 방법이다
* 샤딩을 구성하게 되면 샤드의 수만큼 노드가 존재하며, 서버가 여러 개 존재하므로 부하를 적절히 분산할 수 있다

# Partitioning 전략 : 수평적

> **특정 범위를 기준으로 데이터를 분할한다**

* Range Partition 이라고도 한다
* #### 장점
    * 논리적인 범위의 분산에 효율적
    * 원하는 데이터가 특정 파티션에 모여 있여 관리하기 용이
* #### 단점
    * 데이터의 분포가 고르지 못할 겨웅 데이터 배분 균등하게 불가능
    * ex) 특정 나이대가 많은 경우가 있을 수 있다
    * 이를 해결하기 위해 해쉬 전략을 사용한다

## 해결법 : Hash Partitioning

> **Redis 의 Key 값에 대하여 Hash 함수를 적용한 결과 Redis Master 의 개수 만큼 나머지 연산을 토대로 데이터를 저장할 Master 서버를 지정하는 것**

* Modulo 연산을 통해 데이터 분포 여부와 상관 없이 고르게 분포시킬 수 있다
* 데이터 읽기 요청이 들어온 경우 이 Hash 함수를 적용해서 데이터가 저장된 Master 를 찾아간다

## 해결법 : 재분배, Rebalancing

> **만약 Hash Partitioning 이 적용된 상황에서 데이터 용량이 더 커져서 Master 서버를 추가해야 할 경우 발생하는 문제**

* Server 을 추가하는 경우 Hash 함수의 결과값이 틀려지게 된다
    * 이 경우 재분배를 해주어야 한다
* 재분배를 해주어야 하는데 데이터 재분배는 어떻게 해주어야 할까
* 데이터 재분배 과정에서 굉장히 많은 부하가 발생할 수 있다
* 운영 중에 노드 추가 작업이 자유롭지 못하다

## 해결법 : Consistent Hashing

> **데이터와 더불어 Master 서버에 대하여 동일한 Hash 함수를 적용하고, Master 서버 Hash 값 구간에 해당되는 데이터를 저장한다**

* Rebalancing 부하를 줄이기 위해서 재분배 되는 데이터의 양을 줄여야 한다
* 고려 사항
    * Hash Ring 에 가상 노드를 촘촘하게 그리고 노드가 간격을 균일하게 배치할 수 있도록 알고리즘 적용 필요
* 특정 노드 간의 범위가 벌어짐 = 그만큼 재분배해야 할 데이터의 양이 많아진다

<br>
<br>

# Redis Cluster

> **데이터를 자동으로 여러 개의 Redis Node 에 나누어 저장하는 방법**

* Master 를 **여러 개** 두어 **분산 저장**이 가능하다
    * Master 에 여러 개의 Slave 를 둘 수 있다
* `Scale Out 가능` : 장비를 추가해서 확장할 수 있다는 의미
    * `제한 없는 용량` : 서버를 늘리면 저장 공간이 커진다
* Slave 가 죽어서 복제 노드가 없는 Master 가 생기면 다른 Master 의 여유분을 가지고 빈자리를 채운다
    * 사용자 개입 없이 Cluster 가 알아서 해준다
* Master 가 죽을 시 Slave 가 Master 로 자동 Failover(해결 조치) 된다
* 최소 3 개의 Master 노드가 있어야 구성 가능하다
* 모든 노드가 서로를 감시한다

## Cluster

> **여러 대의 서버를 하나로 묶어서 1 개의 시스템처럼 동작하게 하는 것**

* 여러 대의 서버에 데이터 분산 저장
* 더 빠른 속도로 사용자에게 서비스 제공
* 장애 발생 : 다른 서버로 연결하여 계속해서 서비스 제공

### 장점

* 자동으로 여러 노드에 데이터 분산 저장 가능
* 일부 노드가 죽거나 통신이 되지 않을 때 작업 중단 X

## 동작 방법

> **Redis Cluster 를 지원하는 라이브러리에서 자동으로 처리해준다**

1. 세 개의 Master 가 있다
2. 데이터를 저장할 때 세 곳중 어느 곳에 저장한다
    1. 이때 다른 Master 들은 특정 데이터에 대해 알지 못한다
    2. 그래서 만약 요청이 다른 Master 로 간 경우 그 Master 가 올바른 Master 목적지를 알려준다
3. Client 는 Master 에 요청을 한다

<br>
<br>

# Redis Sentinal

> **Master 와 Slave 를 감시하다가 Master 가 다운되면 이를 감지해서 관리자의 개입 없이 자동으로 Slave 를 Master 로 승격시킨다**

* Master 와 Slave 로 구성

## 기능 : Monitoring

> **Redis Master 와 Slave 들이 제대로 동작하는지 지속적으로 감시한다**

* 감시하다가 문제가 생기면 자동으로 Failover 한다

## 기능 : 자동 장애 조치, Automatic Failover

> **Master 가 다운되었을 때 Slave 를 새로운 Master 로 승격시킨다**

* Slave 가 여러 대 있을 경우 이 Slave 들이 새로운 Master 로부터 데이터를 받을 수 있도록 재구성한다
* 다운된 Master 는 재시작 될 때 Slave 로 전환되어 새로운 Master 를 볼 수 있도록 한다

## 기능 : 알림, Notification

> **감시하고 있는 Redis Instance 들이 Failover 되었을 때 Pub | Sub 으로 Client 에 알리거나 Shell Script 로 관리자에게 이메일이나 SMS 로 알릴 수 있다**

<br>

# Redis Cluster VS Redis Sentinal

# Cluster VS Sentinal

|        비교         | Cluster                                                   | Sentinal                                   |
|:-----------------:|:----------------------------------------------------------|:-------------------------------------------|
|    Master 의 갯수    | 여러 개                                                      | 한 개                                        |
| Master 다운 시 해결 방법 | Master 가 여러 개 있다.<br/>따라서 모든 Master 가 다운된 게 아닌 이상 동작 가능하다 | 다른 Replica를 Master로 승격시켜서 Master 의 일을 대체한다 |
|        장점         | Master가 여러 개이기 때문에 서버의 부하를 나눌 수 있다                        | Master를 한 개로 유지하여 구성하기 편하다                 |
|        구조         | Master - Slave  = One - Many                              | Master - Replica = One - Many              |

<br>
<br>

# 장애 조치, Failover

> **장애 조치 방법이다. 하지만 Redis Cluster 와 Redis Sentinal 과는 다르다**

* 현재 Master 를 Slave 로 바꾸고 Slave 를 Master 로 변경한다
* 이것은 사용자가 직업 오류를 찾고 수행하는 작업이다(Cluster 와 Sentinal 과의 차이점이ㅏㄷ)
* 이 명령은 서버가 클러스터도 아니고 센티널이 모니터 하는 경우도 아닐 때 사용한다

## 개념

* Master 에서 실행한다(복제본에서는 실행할 수 없다)
* 복제본의 IP | Port 를 지정해서 실행할 수 있다
* TIMEOUT 을 지정할 수 있다(지정된 시간 동안 Failover 하지 않으면 롤백)
* Failover 이 오래 걸릴 경우 Failover Abort 로 취소할 수 있다