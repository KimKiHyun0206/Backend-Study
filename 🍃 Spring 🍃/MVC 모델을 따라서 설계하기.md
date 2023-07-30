# 들어가기 앞서서
* Spring 에는 자체적으로 MVC 모델이라는 것이 존재한다
* 하지만 코딩을 하는 과정에서도 MVC 모델을 지켜서 코딩을 하게 되는데 이때 어떻게 해야 할지 고민해 보고 공부하여 기록하자

<br>

# MVC
> Model-View-Controller 의 약자이며 각자 역할을 가진다

* Model : 단순한 자료형
* View : 사용자에게 보여주기 위한 부분
* Controller : Model 과 View 사이에서 중개역을 하며 내부 로직이 어떻게 돌아가는지 큰 흐름을 보여준다
* Service : Controller 의 요청을 받아서 요청을 처리하는 부분이다

## MVC : 규칙
1. Model 은 View 를 알면 안 된다. 이는 반대도 마찬가지이다
2. Model 에는 비즈니스 로직이 있으면 안 된다. 비즈니스 로직은 Service 부분에 있어야 한다
3. Controller 는 Model 과 Service 의 중개역을 하며 이 중개역에는 DTO 를 사용하여 자료를 통신한다
4. Service 는 Model 에 대한 정보를 알 수 있다. 하지만 View 에 대한 것은 알 수 없다

## MVC : 잘못된 사례
### 문제 1 : Model 에 존재하는 비즈니스 로직
```java
public class Lotto{
    
    private List<Number> numbers = new LinkedList();
    
    public Lotto() {
        Set<Integer> nonDuplicateNumber = createNonDuplicateNumbers();
        Iterator<Integer> iterator = nonDuplicateNumber.iterator();
        numbers.add(new Number(true, iterator.next()));
        while (iterator.hasNext()) {
            numbers.add(new Number(false, iterator.next()));
        }
    }
    // 아래에는 위에 사용되는 메소드들이 존재한다
}
```
* 위의 Lotto 라는 클래스는 Model 이다
* 하지만 비즈니스 로직을 생성자 안에 포함하고 있다(생성자 안에 비즈니스 로직이 있는 것도 문제다)
* Model 은 비즈니스 로직을 처리하는 부분이 아니다. 따라서 이 코드를 아래와 같이 수정했다
#### 해결
```java
public class LottoNumberGenerator {

    public Lotto makeNonDuplicateNumbersWhenStart(){
        Set<Integer> nonDuplicateNumber = createNonDuplicateNumbers();
        return new Lotto(makeLotto(nonDuplicateNumber.iterator()));
    }

    private List<Number> makeLotto(Iterator<Integer> iterator){
        List<Number> numbers = new LinkedList<>();
        numbers.add(new Number(true, iterator.next()));
        while (iterator.hasNext()) {
            numbers.add(new Number(false, iterator.next()));
        }
        return numbers;
    }

    private Set<Integer> createNonDuplicateNumbers() {
        Set<Integer> nonDuplicateNumbers = new HashSet<>();
        while (nonDuplicateNumbers.size() <= 6) {
            nonDuplicateNumbers.add((int) (Math.random() * 45) + 1);
        }
        return nonDuplicateNumbers;
    }
}
```
* Service 클래스를 생성하여 이 Service 에서 Model 을 만들 수 있도록 하였다
* 이렇게 하면 Model 에서 비즈니스 로직을 처리하지 않아도 되며
* 비즈니스 로직을 Service 부분으로 이동시킬 수 있다

<br>

### 문제 2 : View 를 아는 Service
```java
public class LengthValidation {
    public boolean lengthCheckValidate(List<String> formula) {
        if(formula.size() % 2 != 0){
            return true;
        }
        new ErrorAlert().alert();
        return false;
    }
}
```
* Validation 은 Service 또는 VO 에서 처리해야하는 것이다
* 그런데 이 부분에 ErrorAlert 라는 View 클래스가 있다
* 이는 Service 가 View 를 아는 것이므로 잘못되었다

#### 해결
```java
public class LengthValidation {
    public boolean lengthCheckValidate(List<String> formula) {
        return formula.size() % 2 != 0;
    }
}
```
* 이렇게 바꾼 다음 바깥에 리턴 값을 받는 함수에서 오류가 발생했다고 리턴을 받으면 ErrorDTO 를 만들어서 Controller 에게 넘기는 방법을 선택했다

<br>

# 마치며
* 더 많이 코딩해보며 MVC 모델에 대하여 공부해 보아야겠다
* 그리고 VO 에 대해서도 더 공부하여 왜 Validation 을 VO 해서 해주는 것이 편하다고 하는지도 알아 보아야겠다