# Criteria

> Criteria는 JPQL의 작성을 도와주는 빌더 클래스이다.

* 문자열로 JPQL을 작성하면 런타임이 되어야 문법 오류를 알 수 있다
* 하지만 Criteria는 자바 코드 기반이기 때문에 안전하게 JPQL을 작성할 수 있다

```
하지만 코드가 복잡해져서 직관적으로 이해하기 어려울 수도 있다
```

```java
class Sample {
    public static main(String[] args) {
        Member member1 = new Member();

        member1.setName("Name#1");
        em.persist(member1);

        Member member2 = new Member();

        member2.setName("Name#2");
        em.persist(member2);
    }
}
```

1. 테스트 데이터로 회원 두 명을 나타내는 Member 엔티티를 영속화시킨다.
2. 단순하게 Member Entity를 조회하는 Criteria 코드를 작성한다

```java
class GetSampleCode {
    void get() {
        //Criteria query builder
        CriteriaBuilder cb = em.getCriteriaBuilder();

        //Criteria 생성, 반환 타입 지정
        CriteriaQuery<Member> cq = cb.createQuery(Member.class);

        Root<Member> m = cq.from(Member.class); //From clause
        cq.select(m); //Select clause

        TypedQuery<Member> query = em.createQuery(cq);
        List<Member> members = query.getResultList();

        members.forEach(member -> {
            System.out.println("member's name: " + member.getName());
        });
    }
}
```

위의 자바 코드는 단순하게 Criteria 절을 이용하여 Member Entity를 모두 조회하는 코드이다

1. CriteriaBuilder를 얻어서 Criteria를 사용할 준비를 마친다
2. 빌더로 CriteriaQuery를 얻는다. 이때 반환형도 설정할 수 있다
3. FROM 절을 생성한다. 반환한 m 값은 Criteria에서 사용하는 별칭이다
4. SELECT 절을 생성한다

<br>

위의 과정을 거치면 JPQL 사용법과 같다.

* createQuery로 TypedQuery를 얻어서 결과를 얻는다

<br>

## WHERE 절과 ORDER BY 절 추가

```java
class WhereAndOrder {
    void sample() {
        //Criteria query builder
        CriteriaBuilder cb = em.getCriteriaBuilder();

        //Criteria 생성, 반환 타입 지정
        CriteriaQuery<Member> cq = cb.createQuery(Member.class);

        Root<Member> m = cq.from(Member.class); //From clause

        //Where clause, m.name = 'Name#2'
        Predicate usernameEqual = cb.equal(m.get("name"), "Name#2");

        //Order by clause, order by id desc
        Order idDesc = cb.desc(m.get("id"));

        //Select clause
        cq.select(m)
                .where(usernameEqual)
                .orderBy(idDesc);

        List<Member> members = em.createQuery(cq)
                .getResultList();

        members.forEach(member -> {
            System.out.println("member's name: " + member.getName());
        });
    }
}
```
* FROM 절  ROOT<Member> 이후에 Where. order by 절을 생성해 주었다.
* m.get("name") 은 JPQL로 m.name 이라는 의미이다
* cb.desc(m.get("id")) 는 JPQL로 m.id desc라고 볼 수 있다

<br>

이렇게 완성한 `Predicate`와 `Order`를 `where`과 `orderBy`에 넣어서 생성해준다
* ROOT는 조회의 시작점
* CriteriaQuery로 Root(from)을 얻는다
* QueryBuilder로 Predicate(where) -> Order(order by)를 생성한다.
* FROM 절을 생성하고 Where과 Order By를 정의했으면 Criteria Query로 조회절을 생성한다

<br>

## ID가 2 이상이면 이름을 역순으로 정렬하는 JQPL을 Criteria로 생성한다
```java
class Sample {
    void sample(){
        Root<Member> m = cq.from(Member.class); //From clause

        //Where clause
        Path<Integer> idPathInteger = m.get("id");
        Predicate userIdGreaterEqualThan = cb.greaterThanOrEqualTo(idPathInteger, 2);

        //Order by clause
        Order nameDesc = cb.desc(m.get("name"));

        //Select clause
        cq.select(m)
                .where(userIdGreaterEqualThan)
                .orderBy(nameDesc);

        List<Member> members = em.createQuery(cq)
                .getResultList();

        members.forEach(member -> {
            System.out.println("member's name: " + member.getName());
        });
    }
}
```
```
Hibernate: 
    /* select
        generatedAlias0 
    from
        Member as generatedAlias0 
    where
        generatedAlias0.id>=2L 
    order by
        generatedAlias0.name desc */ select
            member0_.member_id as member_i1_6_,
            member0_.insert_datetime as insert_d2_6_,
            member0_.update_datetime as update_d3_6_,
            member0_.city as city4_6_,
            member0_.street as street5_6_,
            member0_.zipcode as zipcode6_6_,
            member0_.name as name7_6_ 
        from
            member member0_ 
        where
            member0_.member_id>=2 
        order by
            member0_.name desc
member's name: Name#4
member's name: Name#3
member's name: Name#2
```
