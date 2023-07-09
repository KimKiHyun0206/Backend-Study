# DDL, Database Definition Language
_데이터 정의어_
> 💡 **데이터를 생성, 수정, 삭제하는 등의 데이터의 전체 골격을 결장하는 역할을 하는 언어**

* create : 데이터베이스, 테이블 등을 생성

```mysql
//데이터베이스를 생성한다
CREATE DATABASE sample;

//데이터베이스에 테이블을 생성한다
CREATE TABLE people
(
    name varchar(5),
    age  int
);

CREATE INDEX;
CREATE SCHEMA;
CREATE VIEW;
CREATE USER;
```

* `alter` : 테이블을 수정

```mysql
//ColumnName으로 DataType인 행을 추가한다
ALTER TABLE TableName
    ADD ColumnName DataType;

//ColumnName을 삭제한다
ALTER TABLE TableName
    DROP ColumnName;

//ColumnName의 데이터타입을 DataType로 바꾼다
ALTER TABLE TableName
    ALTER COLUMN ColumnName DataType;
```

* `drop` : 데이터베이스, 테이블 삭제

```mysql
DROP DATABASE Sample;

DROP TABLE People;

DROP SCHEMA
    ...
```

* truncate : 테이블 초기화

```mysql
TRUNCATE TABLE TableName;
```

```
* SCHEMA, DOMAIN, TABLE, VIEW, INDEX를 정의하거나 변경 또는 삭제할 때 사용하는 언어
* 데이터베이스 관리자나 데이터베이스 설계자가 사용
```

# DML, Data Manipulation Language
_데이터 조작어_

> 💡 **정의된 데이터베이스에 입력된 레코드를 조회하거나 수정하거나 삭제하는 등의 역할을 한다**

* `select` : 조회

```mysql
SELECT *
FROM People;
```

* `inset` : 삽입

```mysql
INSERT INTO People
VALUES ("김기현", 21);
```

* `update` : 수정

```mysql
UPDATE People
SET age = 22
WHERE name = "김기현";
```

* `delete` : 삭제

```mysql
DELETE
FROM People
WHERE name = "김기현";
```

```
* 데이터베이스 사용자가 응용 프로그램이나 질의어를 통하여 저장된 데이터를 실질적으로 처리하는 언어
* 데이터베이스 사용자와 데이터베이스 관리 시스템 간의 인터페이스를 제공
```

# DCL, Data Control Language
_데이터 조작어_

> 💡 **데이터베이스에 접근하거나 객체에 권한을 주는 등의 역할을 하는 언어**

* `grant` : 특정 사용자에게 특정 작업에 대한 수행 권한 부여

```mysql
GRANT ALL PRIVILEGES ON DatabaseName.TableName TO userid@'%';

GRANT SELECT, INSERT, UPDATE ON DatabaseName.TableName TO userif@'%' IDENTIFIED BY 'kk020206';
```

* `revoke` : 특정 사용자에게 특정 작업에 대한 수행 권한을 회수

```mysql
REVOKE ALL ON DatabaseName.TableName FROM userid@'%';
```

* `commit` : 트랜잭션의 작업을 저장
* `rollback` : 트랜잭션의 작업을 취소, 원래대로 복구

---
* 7.9 복습