# 데이터베이스 엔진
* 스토리지 엔진(Storage Engine)이라고도 한다
* 데이터베이스 관리 시스템(DBMS)이 DB에 대해 CRUD 잡업을 하는데 사용하는 기본 소프트웨어 컴포넌트
## 사용 방법
* DBMS 고유의 사용자 인터페이스를 이용하는 방법
* 포트 번호를 이용하는 방법


대부분의 DBMS는 DBMS의 사용자 인터페이스를 사용하지 않는다. 사용자가 내장된 엔진과 상호작용을 할 수 있는
자신만의 애플리케이션 프로그래밍 인터페이스(API)를 포함한다

## MySQL의 데이터베이스 엔진이란?
* DB에서 데이터를 어떠한 방식으로 저장하고 접근할 것인지에 대한 기능을 제공
* 엔진 특성에 따라 데이터 접근 속도, 안정성, 트랜잭션 기능 제공 여부 등의 차이점이 있다

### 1. MyISAM
* `MySQL`의 기본 스토리지 엔진
* 데이터 저장에 실제적인 제한이 없다
* 효율적이다
* `Full-Text 인덱스`를 지원하며 특정 인덱스에 대해 메모리 캐시를 지원한다
* 트랜잭션은 지원하지 않는다
* 데드락 예방 가능
* `Table-level Lock` : 테이블작업시 특정 행을 수정하려고 하면 테이블 전체에 락이 걸려 다른 사람이 작업할 수 없다
* SELECT 작업이 많은 경우 사용된다
### 2. InnoDB
* ACID 트랜잭션 지원
* MyISAM보다 데이터 저장 비율이 낮고, 속도가 느리다.
* 특정 데이터와 인덱스에 대해서 메모리 캐시 지원 and 외부키 지원
* 데이터 압축이 불가능하고 자동 에러 복구 기능이 있다
* Row-level Lock : 테이블 레벨이 아닌 ROW 레벨의 락을 지원한다
* INSERT, UPDATE, DELETE 속도가 빠르다
* 대용량 사이트에 적합하다
### 3. Cluster (NDB)
* 트랜잭션 지원
* 모든 데이터와 인덱스가 메모리에 존재
* 속도가 매우 빠름
* PK 사용시 최고의 속도가 나온다
### 4. Archive
* MySQL 5.0부터 새롭게 도입된 엔진
* 자동적으로 데이터 압축 지원
* 다른 엔진에 비해 80% 저장공간 절약
* 가장 빠른 데이터 로드 속도
* INSERT SELECT만 가능하다
### 5. Federated
* MySQL 5.0부터 새롭게 도입된 엔진
* 물리적 데이터베이스에 대한 논리적 데이터베이스를 생성하여 원격 데이터를 컨트롤할 수 있다
* 실행 속도는 네트워크 요소에 따라 좌우된다
* 테이블 정의를 통한 SSL 보안 처리가 가능하다
* 분산 데이터베이스 환경에 사용한다