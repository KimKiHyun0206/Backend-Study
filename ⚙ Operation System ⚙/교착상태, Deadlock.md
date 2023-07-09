# Deadlock

> 두 개 이상의 작업이 서로 상대방의 작업이 끝나기만을 기다리고 있기 때문에
> 결과적으로 아무것도 완료되지 못하는 상태

<div align="center">

<img src="../🔲%20Image%20🔲/리펙토링 이전의 이미지/img2/데드락 예시 이미지1.png">

</div>

# 교착 상태 조건

아래의 조건 중 한 가지라도 만족하지 않으면 데드락이 발생하지 않는다.

## 상호배제, Mutual Exclusion

> 프로세스들이 필요로 하는 자원에 대해 배타적인 통제권을 요구한다

운영체제에서 기본적으로 막아준다.

## 점유대기, Hold and Wait

> 프로세스가 할당된 자원을 가진 상태에서 다른 자원을 기다린다

## 비선점, No Preemption

> 프로세스가 어떤 자원의 사용을 끝낼 때까지 그 자원을 뺏을 수 없다

이미 할당된 자원을 우선순위가 높은 프로세스가 마음대로 가져갈 수 없다.

### 기아현상, Starvation

우선순위가 높은 프로세스가 자원을 계속 가져가면 우선순위가 낮은 프로세스는 언제 실행될 수 있을지 모른다

## 순환대기, Circular Wait

> 각 프로세스는 순환적으로 다음 프로세스가 요구하는 자원을 가지고 있다

* 데드락이 가장 많이 발생시키는 원인
* 위의 그림은 순환대기 데드락이다.

# 교착 상태 관리

교착 상태를 막는 것을 불가능하기 때문에 4가지 조건 중 하나를 관리함으로써 데드락을 예방한다.

## 교착 상태 예방

### 상호배제 예방

두 개 이상의 프로세스가 공유 가능한 자원을 사용할 때 발생하는 것이므로 공유 불가능한.
즉, 상호 배제 조건을 제거하면 교착 상태를 해결할 수 있다.

* 운영체제에서는 이를 해결하기 위해 **Mutex Lock**을 사용한다.

```java
class Sample {
    public static void main(String[] args) {
        Key key = new Key();
        Process p1 = new Process();
        Process p2 = new Process();

        p1.getKey(key);
        //p2.getKey(key); 오류

        p1.returnKey(key);
        p2.getKey(key);    //정상적으로 실행.
    }
}

class Key {
    private boolean isCanGiveKey = true;

    public boolean isCanGetKey() {
        if (this.isCanGiveKey) {
            !isCanGiveKey;
            return ture;
        }
        return false;
    }

    public boolean returnKeyFromProcess() {
        this.isCanGiveKey;
    }
}

class Process {
    private boolean isThisProcessHaveKey = false;

    void processOwnTask() {
        if (isThisProcessHaveKey) {
            //진행
        }
    }

    void getKey(Key key) {
        key.isCanGetKey();
    }

    void returnKey(Key key) {
        key.returnKeyFromProcess();
    }
}
```

### 점유와 대기 예방

> 한 프로세스가 모든 자원을 가질 수 있을 때만 자원을 점유하도록 한다.

```java

@AllArgsConstructor
class Process {
    private final List<Resource> resourceWhichThisProcessNeed;

    boolean resourceCheck() {
        return resourceWhichThisProcessNeed
                .stream
                .allMatch(resource -> resource.canUseResource());
    }

    void processOwnTask() {
        if (resourceCheck()) {
            //작업 진행
        }
    }
}
```

### 비선점 조건 제거

> 다른 프로세스가 가진 자원을 선점할 수 있도록 한다

```java

@Getter
@AllArgsConstructor
class Process {
    private final List<Resource> resourceWhichThisProcessNeed;
    private List<Resource> holdResource;
    private int priority;

    void resourcePreoccupation(Process process) {
        if (isHighPriority(priority)) {
            holdResource.add(process.getResource());
            //작업 진행
        }
    }

    private boolean isHighPriority(Process process) {
        if (this.priority > process.getPriority) {
            return true;
        }
        return false;
    }

    public Resource getResource() {
        //리소스 양도
    }
}
```

### 원형 대기 제거

> 다른 자원을 선점할 수 있게 함으로써 해결할 수 있다

## 교착 상태 회피

> 자원이 어떻게 요청될지에 대한 추가 정보를 제공하도록 요구하는 것이다.
> 시스템에 원형 대기가 발생하지 않도록 자원 할당 상태를 검사하는 것이다.

### 은행원 알고리즘

| Process | Allocation |  Max  | Need  | Available |
|:-------:|:----------:|:-----:|:-----:|:---------:|
|    \    |   A B C    | A B C | A B C |   3 3 2   |
|    A    |   0 1 0    | 7 5 3 | 7 4 3 ||
|    B    |   2 0 0    | 3 2 2 | 1 2 2 ||
|    C    |   3 0 2    | 9 0 2 | 6 0 0 ||
|    D    |   2 1 1    | 2 2 2 | 0 1 1 ||
|    E    |   0 0 2    | 4 3 3 | 4 3 1 ||

* Allocation : 현재 각 프로세스에게 할당된 자원의 수
* Max : 프로세스들이 작업을 완료하기 위해 최종적으로 필요한 자원
* Need : 작업을 완료하기 위해 필요한 자원
* Available : 현재 사용 가능한 자원

### 자원 할당 그래프

<div align="center">

<img src="../🔲%20Image%20🔲/리펙토링 이전의 이미지/img2/데드락- 자원할당그래프.png">

</div>

### Safe Sequence

> 위 두 알고리즘을 사용하여 자원이 제대로 할당되고 데드락이 발생하지 않을 시에
> 프로세스들이 작업을 진행할 수 있도록 하는 것

## 교착 상태 무시

> 예방 혹은 회피기법은 운영체제의 성능에 큰 영향을 미칠 수 있다.
> 때문에 운영체제에 따라서 데드락이 발생하도록 두는 경우가 많다.

이 경우에는 데드락 회복이 필요하다.

* 회복이 예방보다 실행시간이 빠르기 때문에 이와 같은 방법을 사용하는 것이다

## 교착상태 회복

> 위의 4가지 조건 중 하나만 만족시키지 않으면 된다

* `상호배제` : 운영체제에서 알아서 막아준다
* `점유대기` : 우선순위가 낮은 프로세스 or 자원을 많이 가진 프로세스부터 가진 자원을 뱉어낸다
* `비선점` : 다른 프로세스가 가진 자원을 선점할 수 있게 한다
* `순환대기` : 비선점 or 점유대기를 해제하면 자동적으로 해결된다.
