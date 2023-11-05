# Persistence Context
> 엔티티를 영구 저장하는 환경

* 애플리케이션과 데이터베이스 사이에서 객체를 보관하는 가상의 데이터베이스 역할
* EntityManager 를 통해 엔티티를 저장하거나 조회하면 EntityManager 는 Persistence Context 에 Entity 를 보관하고 관리한다

```java
public class Sample{
    public static void main(String[] args) {
        EntityManager entityManager = new EntityManager();
        
        entityManager.persist(new Member("kim",22));
    }
}
```

* EntityManager 는 하나만 만들어진다
* EntityManager 를 통해서 영속성 컨텍스트에 접근하고 관리할 수 있다

<br>

# Persistent Context : Entity`s LifeCycle
* **비영속, new/transient** : 영속성 컨텍스트와 전혀 관계 없는 상태
* **영속, managed** : 영속성 컨텍스트에 저장된 상태
* **준영속, detached** : 영속성 컨텍스트에 저장되었다가 분리된 상태
  * 1차 캐시, 쓰기 지연, 변경 감지, 지연 로딩을 포함한 영속성 컨텍스트가 제공하는 어떠한 기능도 동작하지 않는다
  * 하지만 식별자 값을 가지고 있다
* **삭제, removed** : 삭제된 상태

<center>

![img.png](../🔲%20Image%20🔲/Spring/Persistent%20Context-LifeCycle.png)

</center>

```java
public class Sample{
    public static void main(String[] args) {
        Member member = new Member();   //비영속 상태
        EntityManager entityManager;
        
        entityManager.persist(member);  //영속 상태


        //준영속 상태 예시 3가지
        entityManager.detach(member);   //엔티티를 영속성 컨텍스트에서 분리한다
        entityManager.clear();          //영속성 컨텍스트를 비운다
        entityManager.close();          //영속성 컨텍스트를 종료한다
        
        
        entityManager.remove(member);   //삭제
    }
}
```

<br>

# Persistence Context : 특징

### 영속성 컨텍스트의 식별자 값
* 영속성 컨텍스트는 엔티티를 식별자 값으로 구분한다
* 따라서 영속 상태는 식별자 값이 반드시 있어야 한다

### 영속성 컨텍스트와 데이터베이스 저장
* JPA 는 보통 트랜잭션을 커밋하는 순간 영속성 컨텍스트에 새로 저장된 엔티티를 데이터베이스에 반영한다
* 이를 flush 라고 한다

### 영속성 컨텍스트가 엔티티를 관리하면 다음과 같은 장점이 있다
1. 1차 캐시
2. 동일성 보장
3. 트랜잭션을 지원하는 쓰기 지연
4. 변경 감지
5. 지연 로딩

- - -
* [영속성 컨텍스트](https://velog.io/@neptunes032/JPA-%EC%98%81%EC%86%8D%EC%84%B1-%EC%BB%A8%ED%85%8D%EC%8A%A4%ED%8A%B8%EB%9E%80)