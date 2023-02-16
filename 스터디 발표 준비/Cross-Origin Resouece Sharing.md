# Cross-Origin Resource Sharing, CORS

_교차 출처 리소스 공유_
> 브라우저는 보안적인 이유로 Cross-Origin HTTP 요청들을 제한한다.
> 그래서 Cross-Origin 요청을 하려면 서버의 동의가 필요하다.

* 서버가 동의하면 브라우저는 요청을 허락한다
* 서버가 동의하지 않으면 브라우저에서 거절한다

```
브라우저가 거절하는 것에 주목하자
```

<br>

이러한 허락을 구하고 거절하는 메커니즘을 **HTTP-Header**를 이용해서 가능한데. 이를 **CORS**라고 한다.

<br>

```
브라우저에서 Cross-Origin 요청을 안전하게 할 수 있도록 하는 메커니즘
```

## Cross-Origin

1. **프로토콜** - `http`와 `https`는 프로토콜이 다르다
2. **도메인** - `localhost8080.com` 과 `hanshin.ac.kr`은 다르다
3. **포트 번호** - `localhost:8080` 과 `localhost:3306`은 다르다

# CORS가 필요한 이유

> CORS가 없이 모든 곳에서 데이터를 요청할 수 있게 되면 다른 사이트에서 원래 사이트를 흉내낼 수 있게 된다.

* 기존 사이트와 완전히 동일하게 동작하도록 만들고 로그인 정보를 탈취하는 사이트
* 따라서 이러한 공격을 할 수 없도록 브라우저에서 보호하고, 필요한 경우에만 서버와 합의 하에 요청한다

# 동작 방법

## Simple Requests인 경우

<details>
<summary> Simple Requests란? </summary>

HTTP 메소드가 다음 중 하나여야 한다

* GET
* HEAD
* POST

<br>

자동으로 설정되는 헤더는 제외하고, 설정할 수 있는 다음 헤더들만 변경하면서

* Accept
* Accept-Language
* Content-Language

<br>

Content-Type이 다음과 같은 경우

* application/x-www-form-urlencoded
* multipart/form-data
* text/plain

<br>

이 요청은 추가적으로 확인하지 않고 바로 본 요청을 보낸다

</details>

1. 서버로 요청한다
2. 서버의 응답이 왔을 때 브라우저가 요청한 Origin과 응답한 헤더 Access-Control-Request-Headers의 값을 비교하여 유요한 요청임녀 리소스를 응답한다
    1. 만약 유효하지 않은 요청이라면 브라우저에서 이를 막고 에러를 발생시킨다.

## preflight인 경우

<details>
    <summary>preflight란?</summary>

`Simple Requests`가 아닌 `Cross-Origin`요청은 모두 preflight 요청을 하게 되는데, 실제 요청을
보내는 것이 안전한지 확인하기 위해 먼저 `OPTIONS` 메소드를 사용하여 `Cross-Origin HTTP 요청`을 보낸다.
이렇게 하는 이유는 사용자가 데이터에 영향을 미칠 요청이므로 사전에 확인 후 요청을 보내는 것이다.

<br>
<h3>요청 헤더 목록</h3>

* **Origin**
* **Access-Control-Request-Method**
    * `preflight` 요청을 할 때 실제 요청에 어떤 메소드를 사용할 것인지 서버에 알리기 위해 사용
* **Access-Control-Request-Headers**
    * `preflight` 요청을 할 때 실제 요청에서 어떤 헤더를 사용할 것인지 서버에 알리기 위함

<br>
<h3>응답 헤더 목록</h3>

* **Access-Control-Allow-Origin**
    * 브라우저가 해당 Origin이 자원에 접근할 수 있도록 허용한다
    * 혹은 `*`은 `credentials`이 없는 요청에 한해서 모든 `Origin`에서 접근 가능하도록 허용한다
* **Access-Control-Expose-Headers**
    * 브라우저가 액세스할 수 있는 서버 화이트리스트 헤더를 허용한다
* **Access-Control-Max-Age**
    * 얼마나 오랫동안 `preflight` 요청이 캐싱될 수 있는지 나타낸다
* **Access-Control-Allow-Credentials**
    * `Credentials`가 `true`일 때 요청에 대한 응답이 노출될 수 있는지 나타낸다
    * `preflight` 요청에 대한 응답의 일부로 사용되는 경우 실제 자격 증명을 사용하여 실제 요청을 수행할 수 있는지를 나타낸다
    * 간단한 `GET` 요청은 `preflight`되지 않으므로 자격 증명이 있는 리소스를 요청하면 헤더가 리소스와 함께 반환되지 않으면 브라우저에서 응답을 무시하고 웹 콘텐츠로 반환하지 않는다
* **Access-Control-Allow-Methods**
    * `preflight` 요청에 대한 응답으로 허용되는 메소드들을 나타낸다
* **Access-Control-Allow-Headers**
    * `preflight` 요청에 대한 응답으로 실제 요청 시 사용할 수 있는 HTTP 헤더를 나타낸다

</details>

1. Origin 헤더에 현재 요청하는 Origin과, Access-Control-Request-Method 헤더에 요청하는 HTTP method와
   Access-Control-Request-Header 요청 시 사용할 헤더를 OPTIONS 메소드로 서버에 요청한다.
    1. 이때 내용물은 없고 헤더만 전송한다
2. 브라우저가 서버에서 응답한 헤더를 보고 유효한 요청인지 확인한다
    1. 유요하지 않음 : 요청이 중단되고 에러가 발생
    2. 유효함 : 원래 요청으로 보내려던 요청을 다시 요청하여 리소스를 응답받는다
