# 들어가기 앞서서
요즘 Java 와 JPA 에 대해서 공부하는 도중에 EntityManager 가 정확히 무슨 일을 하고, 어떻게 사용할 수 있는지 공부할 것이다.

<br>

# EntityManager
> 💡 **JPA 에서 Entity 를 저장하고, 수정하고, 삭제하고 조회하는 등 엔티티와 관련된 모든 일을 처리한다**

* 영속성 컨텍스트를 통해 데이터의 상태 변화를 감지하고 필요한 쿼리를 자동으로 수행한다
* Spring Data JPA 에서는 EntityManager 가 자동으로 Bean 이 등록되기 때문에 직접 사용하지 않는다

# EntityManager 와 EntityManagerFactory
* 데이터베이스를 하나만 사용하는 애플리케이션은 일반적으로 `EntityManagerFactory` 를 하나만 생성한다
  * **만드는 비용이 상당히 크기 때문**이다
  * 따라서 **하나만 만들어서 애플리케이션 전체에 공유**하도록 설계되어 있다
  * 반면에 `EntityManagerFactory` 에서 `EntityManager` 를 생성하는 비용은 거의 들어가지 않는다
* 하나의 `EntityManagerFactory` 에서 여러 개의 EntityManager 를 만들 수 있다
* **SpringBoot** 의 경우 `Bean` 에 `EntityManager` 가 자동으로 등록되어 있어서 `@PersistenceContext` 어노테이션으로 사용할 수 있다

> ### ❗ 주의점
> * EntityManagerFactory 는 여러 스레드가 동시에 접근해도 안전하므로 서로 다른 스레드 간에 공유가 가능하다
> * EntityManager 는 여러 스레드가 동시에 접근하면 동시성 문제가 발생하므로 스레드 간에 절대로 공유해서는 안 된다

<br>

# EntityManager : 사용하기
```java
public class Sample{
    public static void main(String[] args) {
        // name에 persistance unit name을 등록할 수 있다.
        EntityManagerFactory emf = Persistance.createEntityManagerFactory("name");
        EntityManager em = emf.createEntityManager();
    }
}
```
이처럼 EntityManagerFactory 에서 EntityManager 를 만든다

## EntityManager : Entity 영속 상태로 만들기
> 💡 **@Entity 어노테이션을 갖는 엔티티 인스턴스를 막 생성했을 때는 영속성 컨텍스트에서 관리하지 않는다. 따라서 등록을 해줘야 한다**

```java
public class Sample{
    public static void main(String[] args) {
        // name에 persistance unit name을 등록할 수 있다.
        EntityManagerFactory emf = Persistance.createEntityManagerFactory("name");
        EntityManager em = emf.createEntityManager();

        em.persist(entity);
    }
}
```

## EntityManager : Entity 준영속 상태로 만들기
> 💡 **영속성 엔티티를 detach 시키거나 영속성 컨텍스트 자체가 초기화 또는 종료되면 컨텍스트 내부의 모든 데이터는 준영속 상태가 된다**
* 관리가 되지 않은 상태로 JPA 의 지원을 받지 않는다
* 하지만 정상적인 데이터를 갖는 인스턴스이다
```java
public class Sample{
    public static void main(String[] args) {
        // name에 persistance unit name을 등록할 수 있다.
        EntityManagerFactory emf = Persistance.createEntityManagerFactory("name");
        EntityManager em = emf.createEntityManager();

        // 1.
        em.detach(someEntity);
        // 2.
        em.close();
        // 3.
        em.clear();
    }
}
```

## EntityManager : Entity 삭제하기
> 💡 **Entity 를 영속성 컨텍스트와 DB 양쪽에서 모두 삭제한다**
```java
public class Sample{
    public static void main(String[] args) {
        // name에 persistance unit name을 등록할 수 있다.
        EntityManagerFactory emf = Persistance.createEntityManagerFactory("name");
        EntityManager em = emf.createEntityManager();

        em.remove(entity);
    }
}
```


<br>

# 마치며
이번에 공부를 하면서 왜 Spring 을 썼을 때 EntityManagerFactory 를 내가 제대로 사용하지 못했는지 알았다. Spring 에서는 EntityManager 를 사용하기 위해서는 `@PersistenceContext`를 사용해야 했기 때문이다. 다음에 사용할 때는 제대로 이 어노테이션을 사용해야겠다.