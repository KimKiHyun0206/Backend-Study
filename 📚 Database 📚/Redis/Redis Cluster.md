# Redis Cluster

> 데이터를 자동으로 여러 개의 Redis 노드에 나누어 저장하는 방법

* Master 를 여러 개 두어 분산 저장 가능하다
* Scale out 가능
* 서버를 늘릴수록 저장할 수 있는 공간이 커진다
    * 제한 없이 커질 수 있다
* Master 에 하나 이상의 Slave 를 둘 수 있다
* Slave 가 죽어서 복제 노드가 없는 마스터가 생길 시 다른 마스터 노드에 여우분이 있다면 해당 노드로 빈자리를 채울 수 있다
    * 이 부분은 사용자 개입 없이 클러스터가 알아서 해준다
* Master 가 죽을 시 Slave 가 Master 로 자동 Failover 된다
* 최소 3개의 Master 노드가 있어야 구성 가능하다
* 모든 노드가 서로를 감시한다

## Cluster

> 여러 대의 서버를 하나로 묶어서 1개의 시스템처럼 동작하게 하는 것

* 여러 대의 서버에 데이터를 분산하여 저장
* 1대의 서버에 부하를 여러 대의 서버로 분산시키므로 더 빠른 속도로 사용자에게 서비스 제공
* 서버에 장애가 발생하게 되면 다른 서버로 연결하여 서비스 중단과 데이터 손실 없이 계속해서 사용자에게 서비스 제공

## 장점

* 자동으로 여러 노드에 데이터를 나누어 저장할 수 있다
* 일부 노드가 죽거나 통신이 되지 않을 때 작업을 계속 할 수 있다

> #### 참고
> * Scale up : 단일 서버의 스팩을 물려 서버의 성능을 높인다
> * Scale out : 서버를 추가하여 서버의 성능을 높인다

## 동작 방법

1. Master 1 2 3이 있다
2. 데이터를 저장할 때 1 2 3 중 어딘가에 저장한다
3. Client 가 데이터 읽기를 요청할 때 저장된 곳이 아닌 다른 Master 에 요청할 수도 있다
4. 만약 잘못 요청했다면 올바른 Master 에 대한 정보를 알려준다
5. Client 는 전달 받은 Master 의 정보에 다시 요청을 해서 데이터를 받는다

> 이 부분은 Redis-Cluster를 지원하는 라이브러리에서 처리해준다


---
#### 🔗
* [Cluster 란?](https://server-talk.tistory.com/502)
* [Redis Cluster란?](https://co-de.tistory.com/24)
* [실습](https://co-de.tistory.com/24)