# Collection
> 데이터의 집합, 그룹을 의미한다.

JSF(Java Collections Framework)는 이러한 데이터, 자료구조인 컬렉션과 이를 구현하는 클래스를 정의하는 인터페이스를 제공한다

# Collection 을 사용하는 이유
1. 일괄된 API : 일관된 API 메소드를 사용하여 Collection 밑의 모든 클래스에 통일된 메소드를 사용한다
2. 프로그래밍 노력 감소 : 객체 지향 프로그래밍의 추상화의 기본 개념이 성공적으로 구현되어 있다
3. 프로그램 속도 및 품질 향상 : 유용한 데이터 구조 및 알고리즘은 성능을 향상시킬 수 있다

# Collection 인터페이스들
## Set
> 순서를 유지하지 않는 데이터 집합으로 데이터의 중복을 허용하지 않는다
* HashSet
* TreeSet

## List
> 순서가 있는 데이터의 집합으로 데이터의 중복을 허용한다
* LinkedList
* Vector
* ArrayList

## Queue
> List 와 유사하지만 선입선출 구조를 가진다
* LinkedList
* PriorityQueue

## Map
> Key 와 Value 의 쌍으로 이루어진 데이터의 집합으로, 순서는 유지하지 않으며 Key의 중복을 허용하지 않으나 Value는 중복이 허용된다
* HashTable
* HashMap
* TreeMap

# 컬렉션과 스트림
> 컬렉션 프레임워크는 자료를 반복적으로 치러하기 위하 Stream 이라는 구문을 지원한다
```java
public class Sample{
    public static void main(String[] args){
        List<Integer> list = new ArrayList();
        
        list.stream().forEach(System.out::println);
    }
}
```
