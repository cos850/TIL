# Kotlin에서 변수를 다루는 방법

> [!NOTE]
> **소스 코드:** 
> [github - lec01: Kotlin에서 변수를 다루는 방법](https://github.com/cos850/java-to-kotlin-starter-guide/tree/master/src/main/kotlin/com/lannstark/lec01)

<br />

## 1. 기본적인 변수 선언

```Kotlin
var number10 = 10L
val number11 = 10L
```
- `var`: 가변
- `val`: 불변
- 타입은 초기화 값으로 추론 

<br />

## 2. 타입 명시
타입스크립트처럼 명시적으로 타입을 지정할 수 있음

```Kotlin
var number20: Long = 10L
```

<br />

## 3. 초기화
- 초기화를 하지 않는 경우, 타입을 명시해야 함 (컴파일러가 타입을 알아야하므로) \
즉, `val number4` 라고 선언하면 오류 발생
- 초기화하지 않은 변수 사용 불가

```Kotlin
val number30: Long
//    println(number4) // number4가 초기화되지 않았기 때문에 오류 !
```

<br />

## 4. Kotlin Primitive Type
- java에서는 Long, long 두 타입이 다르게 표현되지만 \
코틀린에서는 동일하게 사용되며, 실행시 내부적으로 primitive value로써 표현된다.

즉, 자바의

```Java
Long number = 1L
long number = 1L
```

를 코틀린으로 표현하면

```Kotlin
val number: Long = 1L
```

과 같이 동일하게 표현된다. 상황에 따라 코틀린이 알아서 변환한다.

프로그래머가 boxing/unboxing을 고려하지 않아도 된다.

<br />

## 5. kotlin null
Kolin은 null을 가정하지 않는다.

따라서 nullable 변수를 선언하려면, \
`타입 + ?` 로 해당 변수에 null이 들어갈 수 있는 것을 명시해줘야 한다.

```Kotlin
var number50: Long
//    number50 = null   // not null 변수이기 때문에 오류!

var number51: Long?
number51 = null
```

<br />

## 6. kolin 의 인스턴스 생성
kotlin에서는 객체 생성 시 `new` 키워드를 사용하지 않는다.

```Kotlin
val person = Person()
```

<br />

## 7. Tip
1. 모든 변수는 `val`로 만들고
2. 필요한 경우 `var`로 변경하기

<br />

------
### 참조
- [자바 개발자를 위한 코틀린 입문(Java to Kotlin Starter Guide)](https://www.inflearn.com/course/java-to-kotlin/dashboard)
