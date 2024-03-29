# 들어가기 앞서서

인터넷의 가장 중요한 부분이라고 할 수 있는 OSI 7 계층에 대해서 공부해보며, 이 계층을 사용하여 웹에서 어떻게 통신하는지 공부해볼 것이다

<br>

# OSI 7 계층

> 💡 **네트워크에서 통신이 일어나는 과정을 7 단계로 나눈 것**

* **`나눈 이유`** : 통신이 일어나는 과정이 단계별로 파악될 수 있기 때문이다

흐름을 한 눈에 알아보기 쉽고, 사람들이 이해하기 쉽고, 7 단계 중 특정한 곳에 이상이 생기면
다른 단계의 장비 및 소프트웨어를 건들이지 않고도 이상이 생긴 단계만 고칠 수 있기 때문이다

## OSI 7 계층 : 계층 단계

![img.png](../🔲%20Image%20🔲/리펙토링 이전의 이미지/img1/OSI%207계층.png)

![img.png](../🔲%20Image%20🔲/리펙토링 이전의 이미지/img1/OSI 계층 데이터.png)

# OSI 7 계층 : 1 계층 - 물리

> 💡 **이 계층에서는 주로 전기적, 기계적, 기능적인 특성을 이용해서 통신 케이블로 데이터를 전송하게 된다**

| 통신 단위 | 비트                                                       |
|:-----:|:---------------------------------------------------------|
|  기능   | 단순히 데이터를 전달하는 역할만 한다<br/>데이터를 전기적인 신호로 변환해서 주고 받는 기능만 한다 |
| 오류 검출 | X                                                        |
|  장비   | 통신 케이블, 히피터, 허브                                          |

* 통신 단위 : 비트
    * 따라서 1 과 0으로 나타내진다
    * 전기적인 On | Off 상태라고 생각하면 된다

<br>

# OSI 7 계층 : 2 계층 - 데이터 링크

> 💡 **물리 계층을 통해 송수신되는 정보의 오류와 흐름을 관리하여 안전한 정보의 전달을 수행할 수 있도록 돕는다**

| 통신 단위 | 프레임                      |
|:-----:|:-------------------------|
|  기능   | 안전한 정보를 전달할 수 있도록 돕는다    |
| 통신 방법 | MAC 주소를 가지고 통신한다         |
| 오류 검출 | 오류도 찾아주고 재전송도 하는 기능을 가진다 |
|  장비   | 브리지, 스위치                 |

* MAC 주소를 사용하는 이유는 이 계층의 장비가 MAC 주소를 사용하기 때문이다

> ### ❗ 신뢰성 있는 전송
> > Point-To-Point 간 신뢰성 있는 전송을 보장하기 위한 계층이다
> * CRC 기반의 오류 제어와 흐름 제어
> * 네트워크 위의 개체들 간 데이터를 전달하고, 물리 계층에서 발생할 수 있는 오류를 찾아내고, 수정하는데 필요한 기능적, 절차적 수단을 제공한다

> ### ❗ 주소값
> > 물리적으로 할당 받는데, 이는 네트워크 카드가 만들어질 때부터 MAC 주소가 정해져 있다는 뜻이다
> * 주소 체계 : 계층이 없는 단일 구조 (예를 들어 이더넷)
> * 이 외에도 Point-To-Point Protocol, Packet Switching Network, Local Network Protocol 등이 있다
> * 네트워크 브릿지나 스위치 등이 이 계층에서 동작하며, 직접 이어진 곳에만 연결할 수 있다

> ### ❗ 프레임에 주소를 부여하는 이유
> * MAC 은 물리적 주소이다
> * 이는 에러 검출, 재전송, 흐름 제어에 쓰인다

<br>

# OSI 7 계층 : 3 계층 - 네트워크 계층

> 💡 **라우팅을 통해 데이터를 목적지까지 가장 안전하고 빠르게 전달한다**

* 실제 데이터의 전송을 담당한다
* 여러 개의 노드를 거칠 때마다 경로를 찾아주는 역할을 하는 계층이다
* 다양한 길이의 네트워크들을 통해 전달한다
* 이 과정에서 전송 계층이 요구하는 서비스 품질을 제공하기 위한 기능적, 절차적 수단을 제공한다

> ### ❗ Routing
> 1. 경로 선택하고 주소를 정한다
> 2. 경로에 따라 패킷을 전달한다
> * 2 계층 장비 중에서 스위치라는 장비에 라우팅 기능을 장착한 L3 Switch 가 있다

| 통신 단위 | 패킷                                                          |
|:-----:|:------------------------------------------------------------|
|  기능   | 라우팅을 통해 데이터를 목적지까지 안전하게 전달한다<br/>패킷에 IP 를 부여하고, 최적의 경로를 찾는다 |
| 통신 방법 | TCP/IP 상에서 IP 계층을 통해 통신한다                                   |
| 오류 검출 | O                                                           |
|  장비   | 라우터                                                         |
* **패킷** : 전송 계층에서 전달된 세그먼트 헤더에 출발/도착지의 IP 주소를 붙인다


| 네트워크 계층이 수행하는 것 | 설명                                                                                        |
|:---------------:|:------------------------------------------------------------------------------------------|
|       라우팅       | 데이터를 목적지까지 가장 안전하고 빠르게 전달하는 방법을 찾는다                                                       |
|      흐름 제어      | 발신자와 수신자의 전송 속도를 맞추는 것을 의미한다.<br/>너무 빠르게 데이터를 보내면 데이터가 손실될 수 있다.<br/>반대로 너무 느리게 보내면 답답하다. |
|     세그멘테이션      | 받은 데이터를 특정 사이즈로 나누는 기능읗 수행한다.<br/>이 경우 완전한 전송이 되지 않더라고 일부를 볼 수 있게 하고, 데이터 손실률도 줄일 수 있다    |
|      오류 제어      | 오류가 생긴 데이터를 다시 보내준다                                                                       |                                                                                   |

<br>

# OSI 7 계층 : 4 계층 - 전송 계층

> 💡 **통신을 활성화하기 위한 계층이다. 보통 TCP 프로토콜을 이용하며, 포트를 열어서 응용 프로그램들이 전송할 수 있게 한다**

|       통신 단위       | 프레임                                                                                                                     |
|:-----------------:|:------------------------------------------------------------------------------------------------------------------------|
|        기능         | 데이터가 도착하면 이 계층에서 해당 데이터를 하나로 합쳐서 5 계층으로 전달한다                                                                            |
|        특징         | - 특정 연결이 유효성을 제어한다<br/>- 일부 프로토콜은 상태 개념이 있고, 연결 기반이다.<br/>  - 이는 전송 계층이 패킷들의 전송이 유효한지 확인하고 전송 실패한 패킷들은 다시 전송한다는 것을 의미한다 |
|       통신 방법       |                                                                                                                         |
|   오류 검출 및 흐름 제어   | End-to-End 오류 제어 및 흐름 제어를 제공한다<br/>TCP / UCP 프로토콜을 사용한다.                                                                |
|     오류 검출 방법      | 시퀸스 넘버 기반의 오류 제어 방식을 사용한다                                                                                               |
| 오류 검출 및 흐름 제어의 이점 | - 사용자들이 신뢰성 있는 데이터를 주고 받을 수 있다<br/>- 상위 계층들이 데이터 전달의 유효성이나 효율성을 생각하지 않도록 해준다                                            |
|        장비         | 브리지, 스위치                                                                                                                |

* 상위 계층들이 데이터 전달의 유효성이나 효율성을 생각하지 않아도 되는 이유는 라우팅을 통해 최적의 경로와 유효한 경로를 찾았기 때문이다
* 가장 잘 알려진 전송 계층의 예시는 TCP 이다


<br>


# OSI 7 계층 : 5 계층 - 세션 계층

> 💡 **데이터가 통신하기 위한 논리적인 연결을 말한다**

* 하지만 4계층에서도 연결을 맺고 종료할 수 있기 때문에 우리가 어느 계층에서 통신이 끊어졌나 판단하기에는 한계가 있다
* 그러므로 세션 계층은 4계층과 무관하게 응용 프로그램 관점에서 봐야 한다
* 이 계층에서 TCP/IP 세션을 만들고 없애는 책임을 가진다

```
통신하는 사용자들을 통기화하고 오류 복구 명령들을 일괄적으로 다룬다.
통신을 하기 위한 세션을 확립 | 유지 | 중단 = 운영체제가 해준다
```

## 5 계층 - 세션 계층 : 응용 프로그램 관점에서 본 세션 계층
> 💡 **양 끝단의 응용 프로세스가 통신을 관리하기 위한 방법을 제공한다**

* 동시 송수신 방법, 반이중 방식, 전이중 방식의 통신
* 체크 포인팅과 유지, 종류, 재시작 과정 등을 수행한다

### ❗ 세션의 기능
1. 세션 연결의 설정,유지와 해제
2. 세션 중단시 복구 기능
3. 세션 메시지 전송 등의 기능
4. **동기화**

> #### ❗ 동기화
> 동기점이 설정된다는 의미는 그 이전까지의 통신은 서로 완벽하게 처리했다는 것을 의미한다

<br>


# OSI 7 계층 : 6 계층 - 표현 계층

> 💡 **데이터 표현이 상이한 응용 프로세스의 독립성을 제공하고 암호화 한다**

코드 간의 번역을 담당하여 사용자 시스템에서 데이터의 형식상 차이를 다루는 부담을 응용 계층으로부터 덜어준다
* 암호화 등의 동작이 이 계층에서 이루어진다
* 해당 데이터가 어떤 파일 형식인지 구분하는 것도 표현 계층의 몫이다

```
사용자 명령어를 완성 및 결과 표현 = 포장 / 압축 / 암호화
```

<br>

## OSI 7 계층 : 7 계층 - 응용 계층
> 💡 **우리가 사용하는 응용 프로그램을 말한다. 예를 들어 브라우저 등이 있다**

* 최종 목적지로 다양한 프로토콜이 있다(HTTP, SMTP, POP3, IMAP, Talent)
* 아래 계층으로부터 받은 패킷들을 위의 프로토콜에 의해 모두 처리한다
* 처리한 결과를 브라우저나, 메일 프로그램을 사용하여 우리가 프로토콜을 쉽게 응용할 수 있게 해준다

|  통신 방법  | 프로토콜                                                                              |
|:-------:|:----------------------------------------------------------------------------------|
|   특징    | - 응용 프로세스와 직접 관계하여 일반적인 응용 서비스를 수행한다<br/>- 일반적인 응용 서비스는 관련된 응용 프로세스들 사이의 전환을 제공한다 |
| 응용 프로그램 | 우리는 통신을 통해서 패킷을 받고, 브라우저가 패킷을 열어 본다.<br/>그리고 열어본 결과로 브라우저에 화면을 띄우는 것이다            |


<br>
<br>

# 마치며
다시 한 번 응용 계층에 대해서 공부를 하니 헷갈렸던 부분들이 완전히 구분되었다. 물론 내용이 조금 많다 보니 잊어버릴 수도 있지만 그럴 때마다 다시 공부하면 된다.

이번 공부를 하며, 각 계층마다 통신 방법이 어떻게 되는지, 각 계층의 주요 역할이 무엇인지 알게 되었다.

나는 특히 물리 계층의 역할에 대해 이해하는 것이 애매했는데, 이번 복습을 통해서 개념을 더 확실히 알게 된 것 같다.