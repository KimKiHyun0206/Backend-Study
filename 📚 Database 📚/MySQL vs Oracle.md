# MySQL 과 Oracle 의 차이점

* 두 데이터베이스는 모두 RDBMS 이다
* SQL 을 사용한다

<br>

# 차이점

|    차이점    |            MySQL             |                      Oracle                      |
|:---------:|:----------------------------:|:------------------------------------------------:|
|    구조     |  DB 서버마다 독립적인 스토리지를 할당하는 방식  |           DB 서버가 통합된 하나의 스토리지를 공유하는 방식           |
|   조인 방식   |       중첩 루프 조인 방식을 제공        |             중접 루프 조인, 소트 머지 조인 방식 제공             |
|    확장성    |   별도의 DBMS 를 설치해 사용할 수 있다    |             별도의 DBMS 를 설치해 사용할 수 없다              |
|  메모리 사용률  | 메모리 사용률이 낮아서 1MB 환경에서도 설치 가능 |        메모리 사용률이 커서 최소 수백 MB 이상이 되어야 설치 가능        |
|   파티셔닝    | Local Partition Index 만 지원한다 | Local Partition Index, Global Partition Index 지원 |
|   힌트 방식   |  힌트에 문법적 오류가 있으면 오류를 발생시킨다   |        힌트에 문법적 오류가 있으면 힌트를 무시하고 쿼리를 수행한다         |

## SQL 구문의 차이

### NULL 대체

```oracle
NVL
    (열명, '대체값')
```

```mysql
IFNULL
    (열명, '대체값')
```

### SELECT 결과 갯수 제한(페이징 처리)

```oracle
ROWNUM <= 숫자
```

```mysql
LIMIT 시작위치, 가져올 데이터 건수
```

### 가상 테이블 DUAL

```oracle
SELECT 1
FROM DUAL;
```

```mysql
SELECT 1;
```

### 현재 날짜

```oracle
SELECT SYSDATE
FROM DUAL;
```

```mysql
SELECT NOW();
```

### 조건식 (IF)

```oracle
//
칼럼이 값과 일치하면 TRUE, 일치하지 않으면 FALSE
SELECT DECODE(칼럼, 값, TRUE일때 출력할 값, FALSE일때 출력할 값)
FROM TABLE;
```

```mysql
SELECT IFNULL(조건식, TRUE일때 값, FALSE일때 값)
FROM TABLE;
```

### 날짜 형식

```oracle
SELECT TO_CHAR(SYSDATE, 'YYYY-MM-DD')
FROM DUAL;
```

```mysql
SELECT DATE_FORMAT(NOW(), '%Y-%m-%d');
```

### 시퀸스

```oracle
CREATE SEQUENCE [시퀀스명]
    INCREMENT BY [증감숫자]
    START
WITH [시작숫자]
    NOMINVALUE /
MINVALUE [최소값]
NOMINVALUE /
MINVALUE [최소값]
CYCLE /
NOCYCLE
CACHE /
NOCACHE

INSERT TABLE
    (SEQ_NBR)
    VALUES
    (시퀀스명.NEXTVAL)
;
```

```mysql
CREATE TABLE
(
    SEQ_NBR INT NOT NULL AUTO_INCREMENT PRIMARY KEY
);
```

* Insert 시 자동으로 값이 생성되어 들어감

### 문자열 합치기

```oracle
SELECT "A" || "B"
FROM DUAL;
SELECT CONCAT("A", "B")
FROM DUAL;
```

```mysql
SELECT CONCAT("A", "B")
FROM DUAL;
```

### 문자열 자르기

```oracle
SELECT SUBSTR(문자열 / 칼럼, 시작위치, 잘라낼 문자열의 길이)
FROM DUAL;
```

```mysql
SELECT SUBSTRING(문자열 / 칼럼, 시작위치, 잘라낼 문자열의 길이);
```

---

* [MySQL 과 Oracle 의 차이점 1](https://mantaray.tistory.com/38)
* [MySQL 과 Oracle 의 차이점 2](https://velog.io/@alicesykim95/Oracle%EA%B3%BC-MySQL%EC%9D%98-%EC%B0%A8%EC%9D%B4%EC%A0%90)