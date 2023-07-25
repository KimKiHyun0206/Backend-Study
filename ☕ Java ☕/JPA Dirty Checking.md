# JPA Dirty Checking
> **Java 에서 Database 를 사용할 때 상태 변경 검사를 해주는 것**

```java
@Slf4j
@RequiredArgsConstructor
@Service
public class PayService {

    public void updateNative(Long id, String tradeNo) {
        EntityManager em = entityManagerFactory.createEntityManager();
        EntityTransaction tx = em.getTransaction();
        tx.begin(); //트랜잭션 시작
        Pay pay = em.find(Pay.class, id);
        pay.changeTradeNo(tradeNo); // 엔티티만 변경
        tx.commit(); //트랜잭션 커밋
    }
}
```

* EntityManager 를 사용하면 JPA 로 DB 를 조작할 수 있다
* 이때 우리는 별도의 SQL 문을 작성하지 않는다
* 그리고 save 메소드로 변경 사항을 저장하지 않았음에도 update 를 동작하게 할 수 있다
  * JPA 에는 update 메소드만 없다(저장, 조회, 삭제 메소드는 있다)
* 이것이 Dirty Checking 이 해주는 일이다

```
JPA 에서는 트랜잭션이 끝나는 시점에 변화가 있는 모든 앤티티 객체를 데이터베이스에 자동으로 반영해준다
```
* 이때 변화가 있다의 기준은 최초 조회 상태이다

# JPA Dirty Checking : 동작 방법
> ### 동작 조건
> * 영속 상태 엔티티 안에 있는 엔티티인 경우
> * 트랜잭션 안에서 엔티티를 변경하는 경우


1. JPA 는 Entity 를 조회하면 해당 Entity 의 조회 상태 그대로 Snapshot 을 만들어 놓는다
2. 트랜잭션이 끝나는 시점에 이 Snapshot 과 비교해서 다른 점이 있다면 Update Query 를 데이터베이스에 전달한다
    - 이런 상태 변경 검사의 대상은 영속성 컨텍스트가 관리하는 엔티티에만 적용된다
      - 준영속 : Detach 된 엔티티
      - 비영속 : DB 에 반영되기 전에 처음 생성된 엔티티

> ### 주의
> 준영속 / 비영속 상태의 엔티티는 Dirty Checking 대상에 포함되지 않는다. 즉 값을 변경해도 데이터베이스에 반영되지 않는다.

<br>

# Dirty Checking : 변경 부분만 Update 하기
> Dirty Checking 으로 생성되는 Update 쿼리는 기본적으로 모든 필드를 업데이트한다

* JPA 에서는 전체 필드를 업데이트 하는 방식을 기본값으로 사용한다
* 하지만 데이터베이스 필드가 너무 많을 경우 전체 Update 쿼리가 부담스러울 수 있다
  * 정규화가 잘못된 데이터베이스일 확률이 높다
  * 하지만 아닐 수도 있다

> ### 전체 필드 업데이트 방식의 장점
> * 생성되는 쿼리가 같아 부트 실행 시점에 미리 만들어서 재사용 가능하다
> * 데이터베이스 입장에서 쿼리를 재사용할 수 있다
>   * 동일한 쿼리를 받으면 이전에 파싱된 쿼리를 재사용한다

## Dirty Checking : @DynamicUpdate
> 특정 부분의 필드만 업데이트하고 싶은 경우 사용하면 된다

```java
@Getter
@NoArgsConstructor
@Entity
@DynamicUpdate // 변경한 필드만 대응
public class Pay {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String tradeNo;
    private long amount;
}
```

- - -
* [JPA 더티 체킹 1](https://interconnection.tistory.com/121)
* [JPA 더티 체킹 2](https://jojoldu.tistory.com/415)