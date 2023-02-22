# Origin

> 서버의 위치를 의미하는 https://google.com 같은 URL들은 마치 하나의 문자열 같이 보여도 여러 개의 구성 요소로 이루어져 있다.

| **Protocol** |      **Host**      |  **Path**  |   **Query String**   | **Fragment** |
|:--------:|:--------------:|:------:|:----------------:|:--------:|
| https:// | www.google.com | /users | ?sort=asc&page=1 |   #foo   |

이때 출처는 Protocol과 Host 그리고 포트 번호를 모두 합친 것을 말한다

```
Origin = Protocol + Host + Port
```

출처 내의 포트 번호는 생략이 가능한데, 이는 각 웹에서 사용하는 프로토콜의 기본 포트 번호가 정해져있기 때문이다.

<br>

그러나 만약에 `https://google.com:123`과 같이 출처에 포트 번호가 명시적으로 포함되어 있으면
포트 번호까지 모두 일치해야 같은 출처라고 인정한다.

# SOP, Same-Origin Policy

> 같은 출처에서만 리소스를 공유할 수 있다

하지만 웹은 다른 출처의 리소스를 무작정 막을 수도 없다. 따라서 몇가지 예외 조항을 두고 리소스의
출처가 다르더라고 허용을 하기로 했는데 그것이 **CORS**이다.

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

# REST API 와 CORS

> REST API의 리소스가 비 단순 Cross-Origin HTTP 요청을 받는 경우 CORS 지원을 활성화해야 한다

## CORS 지원 활성화 여부 확인

> Cross-Origin HTTP 요청은 다음에 의해 이루어지는 요청이다

* 다른 도메인
* 다른 하위 도메인
* 다른 포트
* 다른 프로토콜

<br>

Cross-Origin HTTP 요청은 단순 요청과 비단순 요청의 두 가지 유형으로 나눌 수 있다

## 단순 요청, Simple Requests

* GET, HEAD, POST 요청만 허용하는 API 리소스에 대해 발행된 요청
* POST 메소드의 경우 Origin 헤더를 포함해야 한다
* 요청 페이로드 컨텐츠 유형이 다음과 같다
    * text/plain
    * multipart/form-data
    * application/x-www-rulencoded
* 요청에 사용자 지정 헤더가 없다

<br>

단순 Corss-Origin POST 메소드 요청의 경우 해당 리소스로부터 응답에 Access-Control-Allow-Origin 헤더가 포함되어야 한다.
헤더의 키 값은 *(임의의 오리진)으로 설정되거나, 해당 리소스에 대한 액세스가 허용되는 오리진으로 설정된다.

<br>

다른 모든 Cross-Origin HTTP 요청은 비단순 요청이다.
API 리소스에서 비 단순 요청을 수신하는 경우 CORS 지원을 활성화해야 한다.

```java

@Configuration
@RequiredArgsConstructor
public class WebConfig implements WebMvcConfigurer {
    @Bean
    public UrlBasedCorsConfigurationSource corsConfigurationSource() {
        var corsConfig = new CorsConfiguration();

        corsConfig.addAllowedOriginPattern(CorsConfiguration.ALL);
        corsConfig.addAllowedHeader(CorsConfiguration.ALL);
        corsConfig.addAllowedMethod(CorsConfiguration.ALL);

        corsConfig.setAllowCredentials(true);
        corsConfig.setMaxAge(3600L);

        var corsConfigSource = new UrlBasedCorsConfigurationSource();
        corsConfigSource.registerCorsConfiguration("/**", corsConfig);
        return corsConfigSource;
    }
}
```

* addAllowedOriginPattern(String originPattern) : Origin을 추가한다
* addAllowedHeader(String allowedHeader) : 허용할 실제 요청 헤더를 추가한다
* addAllowedMethod(String method) : 허가할 HTTP 메소드를 추가한다
    * @Overload : addAllowedMethod(HttpMethod method)

<br>

- - -

* [CORS 스프링 독스](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/cors/CorsConfiguration.html)

