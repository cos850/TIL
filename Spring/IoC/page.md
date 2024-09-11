# 제어의 역전 (IoC, Inversion of Control)

## 제어의 역전

> 💡 프로그램의 제어 흐름에 대한 권한을 외부에서 관리하는 것


제어의 역전 없이 개발을 하게 되면, 클라이언트 구현 객체가 스스로 필요한 서버 구현 객체를 생성하고 실행한다. 이는 구현 객체가 제어의 흐름을 조종하는 권한을 가지고 있는 것이다.

이런 상황에서 구현 객체를 변경하게 되는 경우, 클라이언트 코드에 수정이 발생하게 된다.
<br />

### 예시) 주문 서비스의 할인 정책을 변경하는 상황

할인 정책을 FixDiscountPolicy에서 RateDiscountPolicy로 변경이 필요한 상황이다.

<br />

- **구현 객체가 제어의 흐름을 관리**

```java
public class OrderServiceImpl implements OrderService {

// 정액 할인에서 정률 할인으로 변경하게 되는 경우, 클라이언트 코드에 변경이 발생한다.
//    private final DiscountPolicy discountPolicy = new FixDiscountPolicy();
    private final DiscountPolicy discountPolicy= new RateDiscountPolicy();
}
```

현재 코드가 추상 인터페이스에만 의존하지 않고, 구체 클래스에도 함께 의존하기 때문에 클라이언트코드의 수정이 일어나게 된다!! (OCP, DIP 위반)

이를 해결하려면 **관심사 분리**가 필요하다.

> => 책임을 분리해서 클라이언트는 추상 인터페이스에만 의존하도록 하고, 구체 클래스 할당을 관리해주는 책임을 가진 다른 클래스가 있어야 한다. 

<br />

- **제어의 역전 적용**

생성자를 통해 외부에서 구체 클래스를 받도록 수정한다. (생성자 의존관게 주입)

```java
public class OrderServiceImpl implements OrderService {
    private final DiscountPolicy discountPolicy;

    public OrderServiceImpl(DiscountPolicy discountPolicy) {
        this.discountPolicy = discountPolicy;
    }
}
```

<br />

구현 할당을 관리하는 역할을 새로운 클래스에게 위임한다.

```java

public class AppConfig {

    public OrderService orderService() {
        return new OrderServiceImpl(getDiscountPolicy());
    }

    private static DiscountPolicy getDiscountPolicy() {
        return new RateDiscountPolicy();
    }
}
```

<br />

위와 같이 제어의 역전을 이용하게 되면 

- 구성하는 책임과 실행하는 책임이 분리가 되면서 각 클래스의 책임이 명확해졌다.
(자연스럽게 단일 책임 원칙(SRP)을 지키게 된다)
- 할인 정책이 변경되어도 클라이언트(주문 서비스)에서는 코드를 변경하지 않아도 된다.
- 이제 더이상 클라이언트는 구체 클래스에 대해 알 수 없다!

<br />

> **이렇게 프로그램의 제어 흐름을 직접 제어하는 것이 아니라 외부에서 관리하게 되는 것이 제어의 역전이다.**

<br />

## Spring IoC 컨테이너
`Application Context`는 Spring IoC 컨테이너를 나타내며, 설정 파일에 정의된 내용에 따라 빈을 인스턴스화하는 역할을 한다. 

> 💡 **`BeanFactory`와 `ApplicationContext`**
>
> IoC 컨테이너의 기반이 되는 패키지는 `org.springframework.beans`와 `org.springframework.context` 이다.
>
>- beans 패키지의 `BeanFactory`는 모든 유형의 객체를 관리할 수 있는 메커니즘을 제공한다.
>
>- context 패키지의 `ApplicationContext`는 `BeanFactory`의 하위 인터페이스로, Spring AOP 기능과의 통합을 더 쉽게 해주고, 다국어 처리 등의 추가 기능을 가지고 있다.
>

<br />

Spring Framework에서는 `web.xml`의 구성을 통해 해당 Bean을 정의할 수 있고, Spring Boot에서는 `@SpringBootApplication` 하위에 선언된 `@Component`, `@Service`, `@Repository`, `@Controller` 등을 자동으로 빈으로 등록해준다. 

<br />

## Bean

Spring IoC 컨테이너에 의해 관리되는 객체이다.

컨테이너에 제공하는 구성(`@Bean`, `<bean />`)에 의해 생성되며, 컨테이너 내에서는 `BeanDefinition` 객체로 관리된다.

- **Bean의 재정의**

현재는 빈을 재정의할 수 있지만, 이후 릴리즈에서는 사라질 예정이며, 오버라이딩을 완전히 막기 위해서는 `allowBeanDefinitionOverriding` 플래그를 `false`로 설정하면 된다.

기본적으로는 INFO 레벨의 로그에 재정의된 내용을 출력하기만 하지만, `false`로 설정하면 예외처리를 해준다.

<br />

## 프레임워크 vs 라이브러리

- 나의 코드를 대신 실행하면서 제어하게된다면 그것은 프레임워크
- 반면에 내가 직접 해당 코드를 호출하면서 제어의 흐름을 담당하게 된다면 그것은 라이브러리

즉, 실행의 주도권이 어디에 있는지가 중요한 구분점이 된다는 말이다.

<br />

조금 더 구체적으로 설명하면 다음과 같다.

- 프레임워크는 라이브러리와 다르게 실제 기능을 담당하는 로직은 빠져있으나, 특정 틀을 제공하면서 개발자는 그에 맞춰서 개발을 하면 프레임워크가 호출하는 방식
- 라이브러리는 개발자가 실제 기능을 하는 코드를 직접 호출하여 개발의 편의를 돕는 방식

<br />

---

나는 그동안 라이브러리는 코드만 제공해주는 것, 프레임워크는 라이브러리 + 기본 바탕까지 제공하는것 이라고 알고 있었고, 의미는 알겠지만 잘 와닿지 않았다.


하지만 김영한님의 Spring 핵심 원리 강의에서 제어의 역전을 이용해 간단하게 비교해주신 덕분에 차이점을 명확하게 인지하게 되었다.


**출처**

- https://www.inflearn.com/course/스프링-핵심-원리-기본편
- https://docs.spring.io/spring-framework/reference/core/beans.html
- https://docs.spring.io/spring-boot/reference/using/spring-beans-and-dependency-injection.html