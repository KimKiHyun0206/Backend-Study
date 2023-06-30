# DTO, Data Transfer Object

* 데이터 전송 객체
* 객체지향 언어 환경에서 사용 가능하다
* 개발과 업데이트가 용이하다

## 사용 방법

비즈니스 고직이 아닌 데이터만 저장해야 한다. 그리고 용량이 작은 단순한 환경으로, 한가지 작업만 수행하는 것이어야 한다

```java

@Getter
@Setter
@RequiredArgsConstructor
class PeopleDto {
    private final String name;
    private final int age;
}
```

#### 효과

* 반복되는 코드를 최소화한다
* 작성이 용이하다
* 읽을 수 있다
* 도메인 모델을 캡슐화하여 보호할 수 있다
* 모델과 뷰가 강하게 결합되는 것을 막을 수 있다

## 어디서 사용되어야 할까

> `Controller`와 `Service` 중 어디서 변환되어야 하는가

도메인은 `Service`에서 `DTO`로 변환되어 `Controller로` 전달되어야 한다
