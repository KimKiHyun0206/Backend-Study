# NoSQL

* No Only SQL
* SQL만을 사용하지 않는 DBMS | 비관계형 데이터베이스
    * 데이터 관계를 정의하지 않고 사용하는 DB
* NoSQL은 정해진 규격이 없고, Column이 지정되지 않아 자유롭게 채울 수 있고, 분산 처리도 쉽다
* 트랜잭션 X
* 문서형으론 MongoDB, key-value 형으로 Redis 등이 있다

## SQL(RDS)와의 비교

|   NoSQL    |   SQL   |
|:----------:|:---:|
| Collection | Table |
|  Document  | Row |
| Key, Field | Column |

### RDS(Relation Ddtabase Service)와의 차이점

* 스키마가 없다 : 하나의 Collection에 있는 Document 들은 각각 다른 Field를 가질 수 있다
* 함께 사용하는 객체일 경우, 하나의 Document에 합쳐서 사용한다
* SQL의 JOIN을 사용하지 않는다

### NoSQL을 사용하는 이유

* 데이터가 많이 쌓이는 서비스가 많아지기 때문
* 데이터의 분산처리, 빠른 쓰기 및 데이터의 안정성이 필요할 때 사용
* 특정 서버에 장애가 발생했을 때 데이터 유실이나 서비스 중지가 없는 형태의 구조

<br>

# Redis

* Remote Dictionary Server : 메모리 기반의 Key-Valye 구조의 데이터 관리 시스템
* 비관계형 데이터베이스
* 자바 자료구조와 유사한 영속적인 자료구조 제공
* 읽기 성능 증대를 위한 서버 측 복제 지원
* 쓰기 성능 증대를 위한 클라이언트 측 Sharing 지원
* 싱글 스레드 방식으로 인해 연산을 원자적으로 수행 가능
* 다양한 서비스에서 사영되며 검증된 기술

## Redis의 영속성

Redis는 연속성을 보장하기 위해 데이터를 디스크에 저장할 수 있다. 서버가 내려가더라도 디스크에 저장된
데이터를 읽어서 메모리에 로딩한다. 데이터를 디스크에 저장하는 방식은 두 가지가 있다

* `RDB(Snapshotting)` : 순간적으로 메모리에 있는 내용 전체를 디스크에 옮겨 담는 방식
* `AOF(Append On File)` : Redis의 모든 write/update 연산 자체를 모두 log 파일에 기록하는 형태

## Redis의 컬렉션

Key가 될 수 있는 데이터 구조체가 다양하기 때문에 다양한 자료구조를 지원한다.

* String
* List
* Set
* Stream
* 등 다양한 형태가 있다

## Redis의 장점

* 리스트, 배열 데이터를 처리하는데 유용하다
* 리스트형 데이터 입력 and 삭제가 MySQL에 비해 10배정도 빠르다
* 영속적인 데이터 보존