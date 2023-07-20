# JPA, Java Persistence API

* Java 진영에서 ORM 기술 표준으로 사용하는 인터페이스 모음
* 자바 애플리케이션에서 RDB 를 사용하는 방식을 정의한 인터페이스
* 인터페이스이기 때문에 Hibernate, OpenJPA 등이 JPA 를 구현한다

<div align="center">

![img.png](../🔲%20Image%20🔲/Java/JPA-JPA%20인터페이스.png)

</div>

# JPA : 사용 이유

> 💡 **JPA 는 반복적인 CRUD SQL 을 처리해준다**

* JPA 는 매핑된 관계를 이용해서 SQL 을 생성하고 실행한다
* 개발자는 어떤 SQL 이 실행될지 생각만 하면 되고, 예측도 쉽게 할 수 있다
* 추가적으로 JPA 는 네이티브 SQL 이라는 기능을 제공한다
    * 관계 매핑이 어렵거나 성능에 대한 이슈가 우려되는 경우 SQL 을 직접 작성하여 사용할 수 있다

<br>

> 💡 **JPA 를 사용하여 얻을 수 있는 가장 큰 것은 SQL 이 아닌 객체 중심으로 개발할 수 있다는 것이다**

<div align="center">

![img.png](../🔲%20Image%20🔲/Java/JPA-Java%20ORM%20표준%20프로그래밍.png)

</div>

* 생산성이 좋아지고 유지보수도 수월하다
* JPA 패러다임의 불일치도 해결한다
    * JAVA 에서는 부모 클래스와 자식 클래스의 관계가 존대한다
    * 데이터베이스에서는 이러한 객체의 상속을 지원하지 않는다
        * 지원하는 DB도 있지만 객체 상속과는 다르다
    * 하지만 JPA 는 위와 같은 방식으로 해결한다

<br>

# JPA : 코드

> 위의 구조에서 Album 클래스를 저장한다고 가정할 때

```java
public class Main {
    public static void main(String[] args) {
        jpa.persist(album);
    }
}
```

> JPA 는 코드를 아래의 쿼리로 변환해서 실행한다

```mysql
INSERT INTO ITEM (ID, NAME, PRICE).....
INSERT INTO ALBUM (ARTIST) .....
```

> 조회할 때도 두 테이블을 엮어서 가져온다. 조회하는 JAVA 코드와 변환되는 쿼리

```mysql
// JAVA 코드
String albumId = "id100";
Album album = jpa.find(Album.class, albumId);

// 변환된 쿼리
SELECT I.*, A.*
FROM ITEM I
         JOIN ALBUM A ON I.ITEM_ID = A.ITEM_ID
```

<br>

# JPA : 연관 관계

> 💡 **코드로 따지면 Class 에서 또 다른 Class Type 를 필드 변수로 가지고 있는 것**

<div align="center">

![img.png](../🔲%20Image%20🔲/Java/JPA-연관관계.png)

</div>

```java
class Member {
    String id;
    Team team;
    String username;
}


class Team {
    Long id;
    String name;
}
```

```java
public class Main {
    public static void main(String[] args) {
        Member member = new Member();
        member.setId("1");
        member.setUsername("kim");

        Team team = new Team();
        team.setName("dev_team");

        member.setTeam(team);
        jpa.persist(member);
    }
}
```

```mysql
INSERT INTO MEMBER (ID, TEAM_ID, USERNAME)....
INSERT INTO TEAM (ID, NAME) ....
```

```mysql
// JAVA 코드
Member member = jpa.find(Member.class, memberId);
Team team = member.getTeam();

// 변환된 쿼리
SELECT M.*, T.*
FROM MEMBER M
         JOIN TEAM T ON M.TEAM_ID = T.TEAM_ID 
```

* 위와 같은 구조들이 더 복잡해진다고 해도 JPA 는 이를 모두 지원하기 때문에 문제 없이 사용할 수 있다
* 위에서 다룬 JPA 의 저장 및 조회는 아래와 같은 구조로 실행된다

<div align="center">

![img.png](../🔲%20Image%20🔲/Java/JPA-저장.png)
![img.png](../🔲%20Image%20🔲/Java/JPA-조회.png)

</div>

# JPA : 수정을 지원하지 않는다

> #### 💡 자바 ORM 표준 JPA 프로그래밍 - 김영한
> 반복적인 CRUD SQL 을 작성하고 객체를 SQL 에 매핑하는데 시간을 보내기에는 우리의 시간이 너무아깝다. 이미 많은 자바 개발자들이 오랫동안 비슷한 고민을 해왔고 문제를 해결하려고 많은 노력을 기울여왔다.
> 그리고 그 노력의 결정체가 바로 JPA다. JPA는 표준 명세만 570페이지에 달하고, JPA를 구현한 하이버네이트는 이미 10년 이상 지속해서 개발되고 있으며, 핵심 모듈의 코드 수가 이미 십만 라인을 넘어섰다.
> 귀찮은 문제들은 이제 JPA에게 맡기고 더 좋은 객체 모델링과 더 많은 테스트를 작성하는데 우리의 시간을 보내자. 개발자는 SQL Mapper가 아니다.

* JPA 는 수정 메소드를 제공하지 않는다
* 하지만 당연히 수정은 필요하기 때문에 데이터 수정 시 매핑된 객체(테이블 데이터)를 조회해서 값을 변경 후 커밋하면 DB 서버에 UPDATE 문을 전송하여 UPDATE 를 실행한다
* 일반적인 SQL Mapper 를 사용하는 방법이 더 디테일할 수 있지만 그렇지 않을 수 있다

- - -

* [JPA](https://dbjh.tistory.com/77)






