# Sentinal?

* Redis 는 Master와 Replica로 구성된다
* 운영 중 예기치 않게 Master가 다운되면, 관리자가 이를 감지해서 Replica를 Master로 올리고 Client 들이 새로운 Master에 접속할 수 있도록 해주어야 한다
* Sentinal은 Master와 Replica를 감시하고 있다가 Master가 다운되면 이를 감지해서 관리자의 개입 없이 자동으로 Replica를 Master로 올려준다

## 기능

* 모니터링, Monitoring
* 자동 장애 조치, Automatic Failover
* 알림, Notification

### 모니터링, Monitoring

Sentinal 은 Redis Master 와 Replica 들이 제대로 동작하는지 지속적으로 감시한다

### 자동 장애 조치, Automatic Failover

* Redis Master 가 예기치 않게 다운되었을 때 Replica 를 새로운 Master 로 승격시킨다
* Replica 가 여러 대 있을 경우 이 Replica 들이 새로은 Master 로부터 데이터를 받을 수 있도록 재구성한다
* 다운된 Master 가 재시작 했을 때 Replica로 전환되어 새로운 Master를 바로볼 수 있도록 한다

### 알림, Notification

감시하고 있는 Redis Instance 들이 Failover 되었을 때 Pub | Sub 으로 Client에 알리거나 Shell Script로 관리자에게 이메일이나 SMS로 알릴 수 있다

<br>

# Cluster VS Sentinal

|        비교         | Cluster                                                   | Sentinal                                   |
|:-----------------:|:----------------------------------------------------------|:-------------------------------------------|
|    Master 의 갯수    | 여러 개                                                      | 한 개                                        |
| Master 다운 시 해결 방법 | Master 가 여러 개 있다.<br/>따라서 모든 Master 가 다운된 게 아닌 이상 동작 가능하다 | 다른 Replica를 Master로 승격시켜서 Master 의 일을 대체한다 |
|        장점         | Master가 여러 개이기 때문에 서버의 부하를 나눌 수 있다                        | Master를 한 개로 유지하여 구성하기 편하다                 |
|        구조         | Master - Slave  = One - Many                              | Master - Replica = One - Many              |