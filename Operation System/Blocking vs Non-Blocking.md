# Blocking vs Non-Blocking

동기와 비동기, 블로킹과 논 블로킹의 차이점을 알아볼 것이다

<br>

# 동기 vs 비동기

> 프로세스의 수행 순서 보장에 대한 매커니즘

* 처리해야 하는 작업들을 어떠한 흐름으로 처리할 것이냐에 따른 관점
* 동기, Synchronous : 다른 작업이 끝나는 동시에 시작
* 비동기, Asynchronous : 다른 잡업이 끝나지 않아도 시작한다

<br>

# Blocking vs Non-Blocking

> 작업이 전체적인 작업 흐름을 막는지에 대한 관점

* Blocking : 자신의 작업을 진행하다가 다른 주체의 작업이 시작되면 자신의 작업을 멈추고 해당 작업을 기다렸다가 다시 자신의 작업을 시작한다
* Non-Blocking : 다른 주체의 작업에 관련 없이 자신의 작업을 하는 것

### Blocking 의 예시

* 운영체제의 인터럽트
* 인터럽트는 하던 작업을 멈추고 I/O 작업등 다른 작업을 하기 위해 잠시 작업을 멈춘다
* 이는 블로킹에 해당한다

### Non-Blocking 의 예시

* Database 의 Read 작업
* Database 의 Read 작업은 몇 개를 해도 상관없다
* 이는 다른 주체의 작업에 관련 없이 자신의 작업을 하는 Non-Blocking 의 예시이다
* 반대로 Read 와 Write 는 동시에 이루어질 수 없고 Blocking 하게 된다

<br>

# 조합

> 동기와 비동기, Blocking 과 Non-Blocking 은 개념이 다르기 때문에 조합할 수 있다

## Sync-Blocking

* 동기 : 함수는 다른 함수의 리턴값을 고려해서 동작한다
* Blocking : 함수는 다른 함수에게 제어권을 넘겨주고 대기한다

## Sync-Non-Blocking

* 동기 : 함수는 다른 함수의 리턴값을 고려해서 동작한다
* Non-Blocking : 함수는 다른 함수에게 제어권을 주지 않고 자신의 코드를 계속 실행한다

## Async-Blocking

* 비동기 : 함수는 다른 함수의 리턴 값을 고려하지 않고 동작한다
* Blocking : 함수는 다른 함수에게 제어권을 넘겨주고 대기한다
* Sync-Blocking 과 비교했을 때 이점이 없기 때문에 거의 사용하지 않는다
    * 실수로 많이 사용된다

## Async-Non-Blocking

* 함수는 다른 함수으 리턴 값을 고려하지 않고 동작한다
* 함수는 다른 함수에게 제어권을 주지 않고 자신의 코드를 계속 실행한다

> 함수가 다른 함수를 호출할 때 제어권을 주지 않고 자신의 코드를 계속 실행한다.
> 함수가 다른 함수를 호출할 때 콜백 함수를 함께 줘서 다른 함수는 자신의 작업을 처리하면 콜백 함수를 실행한다
> * ex) AJAX 요청 | JS 비동기 콜백