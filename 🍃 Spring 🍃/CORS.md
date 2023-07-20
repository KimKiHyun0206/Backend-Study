# CORS 성정 시 allowedOrigin 에러 해결 방법

When allowCredentials is true, allowedOrigins cannot contain the special value "*" since that cannot be set on the "
Access-Control-Allow-Origin" response header. To allow credentials to a set of origins, list them explicitly or consider
using "allowedOriginPatterns" instead.


> [스프링 독스](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/cors/CorsConfiguration.html)

## 해결

```java

@Configuration
public class CorsConfig {

    @Bean
    public CorsFilter corsFilter() {

        UrlBasedCorsConfigurationSource source = new UrlBasedCorsConfigurationSource();
        CorsConfiguration config = new CorsConfiguration();

        config.setAllowCredentials(true);
        // config.addAllowedOrigin("*");
        config.addAllowedOriginPattern("*"); // addAllowedOriginPattern("*") 대신 사용
        config.addAllowedHeader("*");
        config.addAllowedMethod("*");
        source.registerCorsConfiguration("/**", config);

        return new CorsFilter(source);
    }
}
```

이와 같은 코드를 작성한다

* `allowCredentials`가 true 일 때 allowedOrigin 에 특수 값인 `*`을 추가할 수 없게 되었다
* 대신 `allowOriginPatterns`를 사용하면 된다

## 메소드

* `void addAllowedHeader(String allowedHeader)` : 허용할 요청 헤더를 추가한다
* `void addAllowedMethod(String method)` : 허용할 HTTP 메소드를 추가한다
* `void addAllowedMethod(HttpMethod method)`
* `void addAllowedOrigin(String origin)` : 허용할 출처를 하나 등록한다
* `void addAllowedOriginPattern(String originPattern)` : 허용할 출처를 등록한다
* `void addExposedHeader(String exposedHeader)` : 노출할 응답 헤더를 추가한다
* `CorsConfiguration applyPermitDefaultValues()` : 기본적으로 CorsConfiguration은 원본 간 요청을 허용하지 않으며 명시적으로 구성해야 합니다.
* `void setAllowedCredentials(Boolean allowCredentials)` : 사용자 자격 증명이 지원되는지 여부
* `void setMaxAge(Lone maxAge)` : 실행 전 요청의 응답을 클라이언트가 캐시할 수 있는 시간(초)을 구성합니다