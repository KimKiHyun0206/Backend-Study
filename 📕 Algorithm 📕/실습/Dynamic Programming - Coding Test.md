# Dynamic Programming
* 메모리제이션 사용
* Bottom-Up 방식의 해결 방법
* 항상 최적해 탐색 가능

<br>

# 경험 : Bottom-Up
* 작은 해부터 구하는 방식을 취하면 당연하게 메모리제이션이 가능해진다
* 작은 해를 어떻게 풀 것인지 점화식을 구하는 것이 가장 중요하다
* 점화식만 제대로 구할 수 있다면 간단하게 풀 수 있다

# 경험 : Memoization
* 구해낸 작은 해를 어딘가에 기록해 놓는 것이다
* 이렇게 해 두면 가장 큰 최적해를 구하고 나머지 최적해를 알고 싶을 때 저장해둔 곳에서 꺼내오면 된다

# 경험 : 최적해
* 작은 해를 구할 때부터 최적해로 구한다
* 따라서 몇 번, 입력값이 무엇이든 최적해가 나온다
* 메모이제이션에 기록된 모든 값이 최적해여야 한다


<br>

# 오류 경험 : 부분 배낭 문제
> ### 2 와 3 으로 숫자 만들기
> > 부분 해가 중복되는 경우를 생각해야 한다
> * 2 와 3의 곱으로 숫자를 만든다고 가정했을 때
> * 6의 배수의 경우 특수한 연산을 해야 한다
> * 6의 배수는 2와 3을 비교해서 더 작은 쪽에 곱을 하는 것으로 연산해야 한다
>   * ex) 2 의 배수의 최소값이 10 이고, 3의 배수의 최소값이 6 일 때 6을 선택할 수 있게 해주어야 한다




