# Local Cache
> 💡 **로컬 장비 내에서만 사요오디는 캐시를 말한다**
* Local 장비의 Resource 를 이용한다
* Memory, Disk

## Local Cache : 장점
* Local 에서 작동되기 때문에 속도가 빠르다
* 네트워크 지연, 단절 이슈에서 자유롭다
* 서버 어플리케이션과 라이프 사이클을 같이 하므로 사용하기 편하다
* 아주 간단한 캐시 등은 메모리 기반으로 동작하는 것이 효율적일 수 있다

## Local Cache : 단점
* 휘발성 메모리 -> 어플리케이션이 다운되면, 메모리 데이터가 사라진다
* 서버 간의 데이터 불일치(여러 서버에서의 메모리 캐시가 일치하지 않는 현상)
* 어플리케이션의 메모리가 부족해지는 현상(서버를 스케일 업 해야 한다)


<br>

# Global Cache
> 💡 **여러 서버에서 Cache Server 에 점근하여 사용하는 캐시**

## Global Cache : 장점
* 데이터를 분산하여 저장할 수 있다
  * Replication : 데이터를 복제
  * Sharding : 데이터를 분산하여 저장
* 별도의 Cache Server 를 이용하기 때문에 서버 간 데이터 공유가 쉽다
* 확장성이 좋다

## Global Cache : 단점
* 네트워크 트래픽으로 인해 Local Cache 에 비해 상대적으로 느리다
* 네트워크 단절 이슈가 있다

> ### ❗ 참고
> > **Cache & Data 배치 전략과 연관이 있다**
> * Read Through
> * Write Back
> * Write Through
> * Write Around


- - -
* [🔗 Local Cache and Global Cache](https://kk-programming.tistory.com/83)