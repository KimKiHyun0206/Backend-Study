# Redis Replication

<br>

## 복제, Replication

> 레디스의 데이터를 거의 실시간으로 다른 레디스 노드에 복사하는 작업

* 서비스를 제공하던 첫 번째 Redis Node 가 다운되더라도, 데이터를 받은 두 번째 Redis Node 가 서비스를 계속할 수 있다
    * 1st Node : Master
    * 2nd Node : Replica

### 만약 복제 기능이 없다면?

> Redis Instance 가 사람의 실수 또는 소프트웨어 적인 문제로 다운되었을 때, 관리자가 이를 인지하고 Redis 를 문제 없이 다시 시작했다면 서비스 중단 시간은 길지 않을 것이다

* 하지만 AOF 기능을 사용하고 있고, 데이터가 많이 쌓여 있다 -> 인스턴스가 시작하는데 오래 걸릴 수 있다
* 다운의 원인이 하드웨어적인 문제였다면 서비스를 다시 시작하는데 상당한 시간이 소요될 수 있다
* 데이터를 복구할 수 없을지도 모른다

### 복제 기능

* 위의 문제를 해결하기 위해 Redis 는 복제기능을 제공한다
* 실제 운영환경이라면 Master-Replica 로 구성할 것을 권장한다
* ### Master 와 Replica 는 물리적으로 다른 머신에 두어야 한다
    * 물리적으로 같은 머신 = 같은 하드웨어
    * 하드웨어에서 보류가 발생 = 두 개 모두 문제가 발생한다
    * Replica 를 둔 이유가 없어진다

<br>

## 복제 구성 설정

#### 복제 서버의 redis.conf

```
# replicaof <masterip> <masterport>
//주석을 지우고 각각의 인수에 Master의 IP와 Port를 넣어준다

replocaof 168.156.0.1 6382
```

* 마스터를 시작한 후 복제 서버를 시작하라
* 마스터에 데이터를 입력하고 복제 서버에서 조회하면 나온다
* 복제는 비동기 방식이긴 하지만 거의 실시간으로 복제 서버에 전달된다

<br>

# Replication 복제

* Redis : 비동기(Asynchronous) 복제를 한다
* Master 는 여러 개의 Replica 를 둘 수 있다
* Replica 는 또 Replica 를 둘 수 있다
* Master 에 많은 데이터가 있는 상태에서 Replica Server 를 시작하면, 대향의 Master 데이터가 Replica 로 보내진다.
    * 이때 Master 는 멈추지 않고 정상적으로 요청을 처리한다
    * Replica 로 데이터를 보내는(RDB 파일을 생성하는) 작업은 자식 프로세스가 처리한다
* Replica 에게 조회 요청을 처리하도록 하는 것은 부하를 분산하는 좋은 방법이다
    * 특히 SORT 명령을 복제 서버에서 수행하는 것이 효율적
* Master 의 부하를 줄이기 위해서, AOF 쓰기나 RDB 파일을 생성해서 Replica 에서 수행하는 것도 좋은 방법이다
    * 하지만 이 경우 Master 가 자동 시작 하도록 하면 데이터가 유실될 수 있다

<br>

# Master 자동 시작과 Persistence 기능 사용

> 서버가 리부트할 때 Redis를 자동 재시작하도록 설정했다면, Master에는 Persistence(AOF, RDB)를 사용할 것을 권장한다.

> Persistence 기능을 사용하지 않는다면 자동 재시작 기능을 이용하지 않는 것이 좋다

* Persistence 기능을 사용하지 않는 상태에서 자동 재시작을 설정하면, Master와 Replica모두 데이터를 읽어버릴 수도 있다

```
1. Node A를 Master(Persistence X)로 하고 Node B를 Replica로 설정
2. Node A가 예외 상황으로 다운되었다가 다시 시작했을 때, 레디스 Master가 자동 재시작.
   Persistence 기능을 꺼 놓았으므로 AOF나 RDB 파일이 없다. 
   그럼 Master는 데이터가 없는 빈 상태
3. Node B는 Master의 아무것도 없는 데이터 복제 = 결과적으로 복제 서버의 데이터도 사라짐.
   Replica에 AOF 파일이 있어도, 초기 복제 후에 AOF Rewrite 기능이 수행되어 Replica의
   AOF 파일에도 데이터는 사라진다
```

> ### 자동 재시작 기능을 사용한다면 Persistence 기능을 꼭 사용하자

<br>

# 복제 방식 - 전체 동기화, Full Synchronization

### 복제 순서

1. Master 는 자식 프로스세를 시작해 백그라운드로 RDB 파일 복제
2. 데이터를 저장하는 동안 Master에 새로 들어온 명량들은 처리 후 복제 버퍼에 저장된다
3. RDB 파일이 저장 완료되면, Master는 파일을 Replica에 전송한다
4. Replica는 파일을 받아 디스크에 저장하고, 메모리로 로드한다
5. Master는 복제 버퍼에 저장된 명령을 Replica에 전송한다

<br>

* 2.8.18 버전부터 RDB 파일을 디스크에 만들지 않고 복제하는 기능을 제공한다
* Master가 다운되면, Replica는 1초에 한 번찍 Master에 Connect 요청을 보낸다
* Master가 살아나면 Replica에 복제 순서에 따라 Sync를 시작한다
* Replica가 여러 개일 때도 RDB 파일은 하나만 생성한다

<br>

# 복제시 서버에서 나오는 정보

### RDB를 디스크에 저장해서 동기화하는 경우(Disk-based) : repl-diskless-sync no

Master

```
59394:M 08:30:53.369 * Slave 127.0.0.1:6383 asks for synchronization
59394:M 08:30:53.369 * Full resync requested by slave 127.0.0.1:6383   복제서버로 부터 전체 동기화 요청을 받음  
59394:M 08:30:53.369 * Starting BGSAVE for SYNC with target: disk
59394:M 08:30:53.370 * Background saving started by pid 59497   자식 프로세스 생성해서 RDB 파일 쓰기를 시작  
59497:C 08:30:54.295 * DB saved on disk   RDB 파일 쓰기 완료  
59497:C 08:30:54.295 * RDB: 42 MB of memory used by copy-on-write
59394:M 08:30:54.325 * Background saving terminated with success
59394:M 08:30:54.34 복제 서버에세 RDB 파일 전송 완료
```

Replica

```
59494:S 08:30:53.369 * Connecting to MASTER 127.0.0.1:6382
59494:S 08:30:53.369 * MASTER <-> SLAVE sync started   마스터에 동기화 요청  
59494:S 08:30:53.369 * Non blocking connect for SYNC fired the event.
59494:S 08:30:53.369 * Master replied to PING, replication can continue...
59494:S 08:30:53.369 * Partial resynchronization not possible (no cached master)
59494:S 08:30:53.369 * Full resync from master: 5ef2944bef28a57b39fb867550b2d02e04f14755:1
59494:S 08:30:54.325 * MASTER <-> SLAVE sync: receiving 33911898 bytes from master   마스터로 부터 RDB 파일을 받기 시작  
59494:S 08:30:54.537 * MASTER <-> SLAVE sync: Flushing old data   메모리에 있는 데이터를 삭제  
59494:S 08:30:54.537 * MASTER <-> SLAVE sync: Loading DB in memory   RDB 파일로 부터 데이터 로드  
59494:S 08:30:55.552 * MASTER <-> SLAVE sync: Finished with success
이후 Appendonly 가 yes 이면 appendonly file Rewrite가 시작된다.
```

### 디스크를 사용하지 않고 소켓을 통해서 동기화하는 경우(Diskless) : repl-diskless-sync yes

Master

```
12637:M 22:43:01.713 * Slave 127.0.0.1:6383 asks for synchronization
12637:M 22:43:01.713 * Full resync requested by slave 127.0.0.1:6383
12637:M 22:43:01.713 * Delay next BGSAVE for SYNC   repl-diskless-sync-delay 만큼 기다림  
12637:M 22:43:07.273 * Starting BGSAVE for SYNC with target: slaves sockets   소켓 통신  
12637:M 22:43:07.274 * Background RDB transfer started by pid 12647
12647:C 22:43:07.406 * RDB: 12 MB of memory used by copy-on-write
12637:M 22:43:07.474 * Background RDB transfer terminated with success
12637:M 22:43:07.474 # Slave 127.0.0.1:6383 correctly received the streamed RDB file.
12637:M 22:43:07.474 * Streamed RDB transfer with slave 127.0.0.1:6383 succeeded (socket). Waiting for REPLCONF ACK from slave to enable streaming
12637:M 22:43:08.057 * Synchronization with slave 127.0.0.1:6383 succeeded
```

Replica

```
12642:S 22:43:07.276 * MASTER <-> SLAVE sync: receiving streamed RDB from master
  이 부분이 disk 방식과 다름. 요청 후 repl-diskless-sync-delay 만큼 기다렸다 이 메시지가 나옴
  Diskless는 마스터에만 적용됩니다. 복제서버는 데이터를 RDB 파일에 저장합니다.  
12642:S 22:43:07.463 * MASTER <-> SLAVE sync: Flushing old data
12642:S 22:43:07.559 * MASTER <-> SLAVE sync: Loading DB in memory
12642:S 22:43:07.755 * MASTER <-> SLAVE sync: Finished with success
```

<br>

# 부분 동기화, Partial Resynchronization

> Master와 Replica는 각 서버의 `run id` 와 `replication offset`을 가지고 있다.
> Master와 Replica 간의 네트워크가 끊어지면 Master는 Replica에 전달할 데이터를 `Backing Buffer`에 저장한다.
> 다시 연결되었을 때 `Buffer`가 넘치지 않았으면 `run id`와 `offset`을 비교해서 그 이후부터 동기화를 시작한다

* `Backing Buffer side` : repl-backlog-size로 설정한다
* 2.8 버전부터 제공한다
* 네트워크 단절 시간이 길어져 Master의 Buffer가 넘치면 다시 연결되었을 때 **전체 동기화**를 한다
* Master나 Replica 중 한 쪽이 재시작했을 경우에도 **전체 동기화**를 한다

<br>

# Master : 디스크를 사용하지 않는 동기화, Diskless Replication

> 디스크를 사용하지 않는 동기화 기능은 Redis를 캐시 용도로 사용할 경우 또는
> Master가 설치된 머신의 디스크 성능이 좋지 않을 경우 이용할 수 있다.

* 디스크를 사용하지 않는 것은 Master만 적용된다
* Replica는 받은 데이터를 RDB 파일에 저장한다
* Master의 자식 프로세스가 RDB 데이터를 소켓을 통해서 Replica에게 직접 쓰는 방식이다
* redis.conf(Master) 파아미터 : repl-diskless-sync {no|yes} (default no)
    * 디폴트는 no 이다
    * yes로 하면 디스크를 사용하지 않고 동기화가 된다
* 여러 Replica에서 요청이 들어올 경우, 기본적으로 첫 번째 Replica의 소켓에 데이터를 전송하고 완려되면 다음 Replica를 처리한다
    * 몇 개의 Replica를 한 번에 처리할 수 있도록 요청을 기다리는 옵션이 있다
* redis.conf(Master) 파라미터: repl-diskless-sync-delay 5
    * 첫 번째 요청이 온 후 5초동안 다른 Replica의 요청을 기다렸다가 요청이 오면 같이 처리한다
    * 5초 안에 3개의 Replica에서 동기화 요청이 오면 이는 병렬로 처리할 수 있따
    * 즉시 처리 = 0

<br>

# Replica : 디스크를 사용하지 않는 동기화, repl-diskless-load

> Replica에서 디스크를 사용하지 않는 동기화.
> 복제 서버에 RDB 파일을 생성하지 않는다

* 6.0부터 지원
* redis.conf(Replica) 파라미터: repl-diskless-load disabled/on-empty-db/swapdb , default disabled 세 가지
    * disabled : diskless를 사용하지 않는다
    * on-empty-db : Replica에 데이터(키)가 없을 경우에 적용. 데이터가 있으면 RDB 파일을 생성해서 복제
    * swapdb : 복제 서버에 데이터(키) 여부와 상관엇이 diskless로 동작한다.
        * 이 경우 만약을 대비해 기존 데이터를 RAM에 보관한다.
        * 성공 : RAM에 보존한 데이터 삭제
        * 실패 : RAM에 보존한 데이터로 복구
        * 이 경우 기존 데이터 + 새 데이터 만큼 RAM이 필요하므로 충분한 메모리가 있어야 한다

<br>

# Replica는 읽기 전용

* 2.6부터 Replica Server는 디폴트로 읽기 전용이다
* redis.conf 파라미터 : replica-read-only yes|no (default yes)
* Replica Server에 데이터를 입력했어도 마스터와 Resync 되면 복제 서버에 입력된 데이터는 사라진다

<br>

---

* [Redis Replication 참고](http://redisgate.kr/redis/configuration/replication.php)








