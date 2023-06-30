# SQL Injection

> 악의적인 사용자가 보안상의 취약점을 이용히여, 임의의 SQL문을 주입하고 실행되게 하여
> 데이터베이스가 비정상적인 동작을 하도록 조작하는 행위

* OWASP Top 10 중 첫 번째에 속해있다
* 공격이 비교적 쉬운 편이고 공격에 성공할 경우 큰 피해를 입힐 수 있다

## 공격 종류 및 방법

### Error based SQL Injection, 논리적 에러를 이용한 SQL Injection

> 가장 많이 쓰이고, 대중적인 공격 기법

```sql
SELECT *
FROM Users
WHERE id = 'INPUT1'
  and password = 'INPUT2';


SELECT *
FROM Users
WHERE id = ''
   OR 1 = 1 -- OR 1=1' AND Password = ' INPUT2'

SELECT *
FROM Users --에러로 인해 이렇게 바뀐다
```

에러로 인해 아래로 바뀌게 된다

* 매우 간단한 구문이지만, Users 테이블의 모든 정보를 조회하게 된다
* 이로 인해 개인정보 유출 등의 사건이 일어난다
* 이로 관리자의 계정을 탈취하면 2차 피해가 발생한다

<br>

### Union based SQL Injection, Union 명령어를 이용한 SQL Injection

> Union 키워드는 두 개의 쿼리문에 대한 결과를 통합해서 하나의 테입르로 보여주게 하는 키워드이다.
> 정상적인 쿼리문에 Union 키워드를 사용하여 인젝션에 성공하면, 원하는 쿼리문을 실행할 수 있게 된다

#### 성공시키기 위한 두 가지 조건

1. Union 하는 두 테이블의 컬럼 수가 같아야 한다
2. 두 테이블의 데이터 형이 같아야 한다

<br>

```sql
SELECT *
FROM Board
WHERE title LIKE '%INPUT%'
   OR contents '%INPUT2%';

UNION
SELECT null, id, passwd
FROM Users -- 그 외 구문

SELECT *
FROM Board
WHERE title LIKE '%'
UNION
SELECT null, id, passwd
FROM Users --그 외 구문
```

* 이 인젝션은 사용자의 id와 passwd를 요청하는 쿼리문이다
* 패스워드가 평문으로 데이터베이스에 저장되지는 않지만 인젝션이 가능한 시점에서 보안에 위험이 있다

<br>

## Blind SQL Injection,

### Boolean based SQL

> 데이터베이스로부터 특정한 값이나 데이터를 전달받지 않고, 단순히 참과 거짓의 정보만 알 수 있을 때 사용한다

* 로그인 폼에 SQL Injection이 가능하다고 가정했을 때, 서버가 응답하는 로그인 성공과 로그인 실패 메시지를 이용하여, DB의 테이블 정보 등을 추출해낼 수 있다

```sql
SELECT *
FROM Users
WHERE id = 'INPUT1'
  AND password = 'INPUT2';

SELECT *
FROM Users
WHERE id = 'abc123'
  AND ASCII(
              SUBSTR(
                  SELECT name 
                  From information_schema.tables 
                  WHERE table_type=’base table’ limit 0,1
                  )1,1
          ) > 100
```

이걸 로그인이 될 때까지 시도하는 것이다

* 위 구문은 MySQL에서 테이블명을 조회하는 구문으로 limit 키워드를 통해 하나의 테이블만 조회하고 SUBSTR 함수로 테이터를 조회한다
* 참이 될 때까지 이걸 하며 공격자는 이 프로세스를 자바스크립트로 자동화해 단기간에 테이블명을 알아낼 수 있다

<br>

## Time based SQL

> 서버로부터 특정한 응답 대신에 참 혹은 거짓의 응답을 통해서 데이터베이스의 정보를 유추한다.
> 사용되는 함수는 MySQL 기준 SLEEP과 BENCHMARK이다

```sql
SELECT *
FROM Users
WHERE id = 'INPUT1'
  AND password = 'INPUT2';

SELECT *
FROM Users
WHERE id = 'abc123'
   OR (LENGTH(DATABASE()) = 1 AND SLEEP(2));
```

* `LENGTH` : 함수의 문자열 길이를 반환한다
* `BATABASE` : 데이터베이스 이름을 반환한다
* SLEEP(2)가 참이면 동작하고 거짓이면 동작하지 않는다
* 숫자 1 부분을 조작하여 데이터베이스의 길이를 알아낼 수 있따

<br>

## Stored Procedure SQL Injection, 저장된 프로시저에서의 SQL Injection

> 저장 프로시저는 일련의 쿼리들을 모아 하나의 함수처럼 사용하기 위한 것.
> 공격에 사용되는 대표적인 저장 프로시저는 MS-SQL에 있는 xp_cmdshell로 윈도우 명령어를 사용할 수 있게 된다

단 공격자가 시스템 권한을 획득해야 하므로 공격 난이도가 높으나 공격에 성공하면, 서버에 직접적인 피해를 입힐 수 있따

<br>

## Mass SQL Injection, 다량의 SQL Injection 공격

> 한 번의 공격으로 다량의 데이터베이스가 조작되어 큰 피해를 입히는 것.
> 악성 스크립트를 삽입하고, 사용자들이 변조된 사이트에 접속하여 좀비 PC로 만들어진다.
> 이렇게 감염된 PC 들이 DDoS 공격에 사용된다

<br>

# 대응 방안

## 입력한 값에 대한 검증

* SQL Injection에 사용되는 기법과 키워드는 굉장히 많다
* 사용자의 입력 값에 대한 검증이 필요한데 서버 단에서 화이트리스트 기반으로 검증해야 한다
* 블랙리스트 기반으로 검증하게 되면 수많은 차단리스트를 등록해야 하고 하나라도 빠지면 성공하게 된다

<br>

## Prepared Statement 구문 사용

* 사용자의 입력 값이 데이터베이스 파라미터로 들어가기 전에 DBMS가 미리 컴파일하여 실행하지 않고 대기한다
* 그 후 사용자의 입력 값을 문자열로 인식하게 하여 공격 쿼리가 들어간다고 하더라도, 사용자의 입력은 이미 의미 없는 단순 문자열이기 때문에 전체 쿼리문도 공격자의 의도대로 작동하지 않는다

<br>

## Error Message 노출 금지

* 에러 메시지가 노출되면 테이블명, 컬럼명, 쿼리문이 노출될 수 있다
* 이를 막아서 사용자에게 대신 보여줄 페이지 혹은 메시지박스를 만들어야 한다

<br>

## 웹 방화벽 사용

* 웹 방화벽은 소프트웨어형, 하드웨어형, 프록시형 세 가지 종류가 있다
* `소프트웨어` : 서버 내에 설치하는 방법
* `하드웨어` : 네트워크 상에서 서버 앞 단에 직접 하드웨어 장비로 구성하는 것
* `프록시` : DNS 서버 주소를 웹 방화벽으로 바꾸고 서버로 가는 트래픽이 웹 방화벽을 거치도록 한다
