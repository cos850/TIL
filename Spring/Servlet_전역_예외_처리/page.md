# Servlet 전역 예외 처리
스프링 프레임워크에서는 전역 에러 처리를 위한 인터페이스를 제공한다.

| servlet (webmvc) | `HandlerExceptionResolver` |
| --- | --- |
| reactive (webflux) | `WebExceptionHandler` |

<br />

## 1. HandlerExceptionResolver

> 💡 Servlet 에서 전역 예외 처리를 하기 위해 제공되는 인터페이스

<br /> 

## 2. HandlerExceptionResolver 구현체

| HandlerExceptionResolver | Description |
| --- | --- |
| SimpleMappingExceptionResolver | 발생한 exception class 이름과 error view 이름을 맵핑해서 사용 <br /> - 기본 설정대상이 아님 (아래 3 클래스는 기본으로 등록된다) <br /> - 각 exception에 대한 에러 페이지 처리를 할 수 있다 |
| DefaultHandlerExceptionResolver | 기본적으로 사용되는 exception 들을 공통 처리해주기 위해 제공되는 resolver <br /> - 스프링이 알아서 처리해주는 에러 처리 모음 = 따로 구현할 필요 없음 |
| ResponseStatusExceptionResolver | ResponseStatusException이 throw 되거나 @ResponseStatus를 설정한 지점에서 발생한 에러를 처리 <br /> - 한 에러에 종속되거나 어노테이션을 메서드나 클래스에 지정해야하기 때문에 전역적으로 사용하기엔 무리가 있다. |
| ExceptionHandlerExceptionResolver | @Controller 또는 @ControllerAdvice에 선언된 @ExceptionHandler 을 통해 에러를 처리 <br /> - Controller/RestController를 사용하는 경우 전역적으로 예외를 처리하기 편하다. |

<br />

여러 개의 `HandlerExceptionResolver` 를 정의해 `order` 프로퍼티가 높을 수록 나중에 적용되도록 설정할 수 있다.

`HandlerExceptionResolver` 는 아래와 같은 것을 반환할 수 있다.

- `ModelAndView` 오류 페이지 반환
- 빈 `ModelAndView`반환
- `null` (모든 resolver 에서 예외가 남아있으면 서블릿 컨테이너까지 전달됨)

<br />

모든 `HandlerExceptionResolver` 에 예외가 남아있거나, 응답 상태코드가 4xx, 5xx 일 경우, 서블릿 컨테이너는 기본 HTML 오류 페이지를 랜더링할 수 있다. 오류 페이지는 `web.xml`에서 아래와 같이 설정할 수 있다.

```xml

<!-- default -->
<error-page>
    <location>/error</location>
</error-page>

<!-- 특정 error code 설정 -->
<error-page>
	<error-code>404</error-code>
	<location>/WEB-INF/html/error/404error.jsp</location>
</error-page>

<error-page>
	<error-code>500</error-code>
	<location>/WEB-INF/html/error/500error.jsp</location>
</error-page>
```

<br />

설정한 URL은 DispatcherServlet 에 의해 @Controller와 매핑되어 view name을 반환하거나 JSON 응답을 반환할 수 있다. 

```java
@RestController
public class ErrorController {

    @RequestMapping(path = "/error")
    public Map<String, Object> handle(HttpServletRequest request) {
        Map<String, Object> map = new HashMap<String, Object>();
        map.put("status", request.getAttribute("javax.servlet.error.status_code"));
        map.put("reason", request.getAttribute("javax.servlet.error.message"));
        return map;
    }
}
```

---

**참조**

[spring docs: Web on Servlet Stack](https://docs.spring.io/spring-framework/docs/5.1.6.RELEASE/spring-framework-reference/web.html#mvc-exceptionhandlers)