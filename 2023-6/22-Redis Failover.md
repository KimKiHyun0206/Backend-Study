# Redis Failover
* 현재 마스터를 복제본으로 바꾸고 복제본을 마스터로 변경한다
* Cluster Failover | Sentinal Failover 와 비슷한 기능을 한다
* 이 명령은 서버가 클러스터도 아니고 센티널이 모니터하는 경우도 아닐 때 사용한다

# 설명
* 마스터에서 실행한다(복제본에서는 실행할 수 없다)
* 복제본의 IP, Port 를 지정해서 실행할 수 있다
* TIMEOUT 을 지정할 수 있다(단위는 milliseconds)
  * 지정한 시간동안 Failover 되지 않으면 롤백한다
* Failover 시간이 오래 걸릴 경우 Failover Abort 로 취소할 수 있다
* info replicaion 의 master_failover_state
  * no-failover : 진행 중인 장애 조치(Failover)가 없다
  * waiting-for-sync : 마스터는 복제본이 데이터가 복제되기를 기다리고 있따
  * failover-in-progress : 마스터가 자신을 복제(replica)로 만들었고 복제본이 새 마스터가 되기를 기다리고 있다
```
127.0.0.1:6010> role
1) "master"
127.0.0.1:6000> failover
OK
127.0.0.1:6010> role
1) "slave"
```
> #### 주의
> 복제본에 여러 개일 경우 복제본 중 하나가 마스터가 되고, 
> 명령을 실행한 마스터는 복제본이 되고 새로운 마스터를 바라본다.
> 그런데 나머지 복제본은 새 마스터를 바라보도록 변경되지 않고 이전 마스터를 바라본다

---
* [Redis Korea](http://redisgate.kr/redis/server/failover.php)