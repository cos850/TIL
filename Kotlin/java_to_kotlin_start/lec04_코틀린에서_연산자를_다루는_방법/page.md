# 코틀린에서 연산자를 다루는 방법

> [!NOTE]
> **소스코드**: 
> [lec04: 코틀린에서 연산자를 다루는 방법](https://github.com/cos850/java-to-kotlin-starter-guide/tree/master/src/main/kotlin/com/lannstark/lec04)

## 비교 연산자
Java와 다르게 객체를 비교할 때 비교 연산자를 사용하면 자동을 `compareTo` 호출

```kotlin
var money1 = JavaMoney(100L)
var money2 = JavaMoney(90L)

if(money1 > money2)
    println("money1 이 money2 보다 큽니다.")
else
    println("money1 이 money2 보다 작습니다.")
```
<br />

### 동등성 / 동일성

> [!TIP]
> **동등성 / 동일성**
> - **동등성**: 값이 같은가
> - **동일성**: 주소값이 같은가

<br />

|구분|동일성|동등성|
|--|--|--|
|Java|`==`|`equals()`|
|Kotlin|`===`|`==` 사용 시 내부적으로 `equals()` 호출|

<br />

**동등성 / 동일성 예시**
```kotlin
money2 = JavaMoney(100L)
if (money1 == money2)
    println("money1 과 money2 는 동일한 값을 가지고 있습니다.")

money2 = money1
if (money1 === money2)
    println("money1 과 money2 는 주소값이 동일합니다.")
```

<br />

### Lazy evaluation

> [!TIP]
> **Lazy evaluation** \
> : 실제로 필요해 질 때 연산을 수행하는 것. \
> 코틀린과 자바 모두 동일하게 적용된다.

<br />

**Lazy 예시**
```java
public static void main(String[] args) {
   System.out.println(lazy("AAA") || lazy("ABC"));

 public static boolean lazy(String s) {
   System.out.println("call lazy()");
   return s.startsWith("A");
 }

// 출력
// call lazy()
// true
```

<br />

## 코틀린 연산자
### `in` / `!in`
: 컬렉션이나 범위에 포함되어 있다 / 포함되어 있지 않다.

### `a..b`
: a부터 b까지의 범위 객체를 생성한다.


<br />

## 연산자 오버로딩
코틀린에서는 객체마다 연사자를 직접 정의할 수 있다.

예를 들어, 코틀린 클래스에 plus()함수를 재정의한 경우
```kotlin
data class KotlinMoney(val amount: Long) {
    operator fun plus(other: KotlinMoney): KotlinMoney {
        return KotlinMoney(this.amount + other.amount)
    }
}
```
<br />

아래와 같이 사용할 수 있다.
```kotlin
val money03 = KotlinMoney(10L)
val money04 = KotlinMoney(20L)
println("money03 + money04 = ${money03 + money04}") // money03 + money04 = KotlinMoney(amount=30)
```

------
### 참조
- [자바 개발자를 위한 코틀린 입문(Java to Kotlin Starter Guide)](https://www.inflearn.com/course/java-to-kotlin/dashboard)