# EntityManager

> Entity 를 저장하는 가상의 DB

<br>

## JPA 의 코드는 세 부분으로 나뉘어 있다

1. EntityManager 성절
2. 트랜잭션 관리
3. 비즈니스 로직

<br>

## JPA 가 제공하는 기능

1. Entity 와 테이블을 매핑하는 설계 부분
2. 매핑한 데이터를 실제 사용하는 부분

<br>

## EntityManager 가 하는 일

> CRUD

Entity 를 저장하는 가상의 DB 라고 생각할 수 있따

<br>

DB를 하나만 사용하는 애플리케이션은 일반적으로 `EntityManagerFactory`를 하나만 생성한다

```java
class Sample {
    public void sampleCode() {
        EntityManagerFactory entityManagerFactory =
                Persistence.craeteEntityManagerFactory("해당 이름");

        EntityManager entityManager = entityManagerFactory.createEntityManager();
    }
}
```

이 방식으로 작성하면 생성된 EntityManagerFactory를 통해 필요할 때마다 EntityManager를 생성할 수 있다

<br>

여기서 중요한 점은 `EntityManagerFactory`는 **여러 스레드가 동시에 접근해도 안전하다는 것**이다. 하지만
`EntityManager`는 **여러 스레드가 동시에 접근하면 동시성 문제가 발생**한다.

# 영속성 컨텍스트

```yml
spring:
  servlet:
    multipart:
      max-file-size: 10MB
      max-request-size: 10MB
  datasource:
    url: jdbc:mysql://localhost:3306/se_todo?useUnicode=true&charset=utf8mb4&characterEncoding=utf8&zeroDateTimeBehavior=convertToNull&serverTimezone=Asia/Seoul&useSSL=false
    driver-class-name: com.mysql.cj.jdbc.Driver
    username: root
    password:
    hikari:
      minimum-idle: 10
      maximum-pool-size: 20
```

이러한 방법으로 데이터베이스를 연결한다

<br>

이렇게 생성된 정보들은 영속성 컨텍스트라는 곳에 저장한다.

* 영속성 컨텍스트 : 엔티티를 영구 저장하는 환경

<br>

## 영속성 컨텍스트의 4가지 상태

1. **비영속** : 영속성 컨텍스트와 전혀 관계가 없는 상태
2. **영속**  : 영속성 컨텍스트에 저장된 상태
3. **준영속** : 영속성 컨텍스트에 저장되었다가 분리된 상태
4. **삭제** : 삭제된 상태

