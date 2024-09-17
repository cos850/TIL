# 코틀린 이모저모

> [!NOTE]
> **소스코드**: 
> [lec19: 코틀린 이모저모](https://github.com/cos850/java-to-kotlin-starter-guide/tree/master/src/main/kotlin/com/lannstark/lec19)


<br />

 
## typealias
: 코틀린의 타입을 대신 표현할 수 있는 별칭 같은 것!

주로 너무 긴 함수 타입을 대신 표현하는 데 사용하거나, \
긴 이름의 컬렉션을 간단하게 표현할 때도 사용한다.

```kotlin
// 함수 타입
typealias FruitFilter = (Fruit) -> Boolean
fun filterFruits(fruits: List<Fruit>, filter: FruitFilter) { /** ... */ }

// 긴 이름의 Collection
data class UltraSuperGuardianTribe(val name: String)
typealias USGTMap = Map<String, UltraSuperGuardianTribe>
```

<br />

## as import
한 파일에서 다른 패키지의 동일한 이름의 함수를 모두 사용하고 싶을 때 사용한다.

```kotlin
import com.lannstark.lec19.a.printHelloWorld as helloWorldA
import com.lannstark.lec19.b.printHelloWorld as helloWorldB

fun main() {
    helloWorldA()
    helloWorldB()
}
```

<br />

## 구조 분해
복합적인 값을 분해해서 여러 변수를 한 번에 초기화 하는 것

```kotlin
data class Person(val name: String, val age: Int)

fun main {
    val person = Person("철수", 5)
    val (name, age) = person

    println("name = ${name}, age = ${age}")
}
```
<br />

위의 구조분해 할당한 코드를 풀어서 쓰면 아래와 같다.

```
//    val (name, age) = person
    val name = person.component1()  // person의 첫번째 프로퍼티를 가져옴
    val age = person.component2()   // person의 두번째 프로퍼티를 가져옴
```

componentN은 객체의 프로퍼티를 순서대로 반환한다.

data class는 이 componentN 이라는 함수도 자동으로 만들어 준다!

따라서 일반 클래스로 구조분해 하려고 하면 component1, 2를 만들면 된다.

```kotlin
class Person(val name: String, val age: Int) {
    operator fun component1(): String {
        return this.name
    }
    operator fun component2(): Int {
        return this.age
    }
}
```

<br />

## Jump, Lable

람다의 foreach 에서는 break나 continue와 같은 제어문을 사용할 수 없다.

만약 사용하고 싶은 경우 아래와 같이 구현할 수 있다.
```kotlin

val numbers = listOf(1, 2, 3)
run {
    numbers.forEach {num ->
        if (num == 2) {
            return@run  // break와 동일한 역할
        }
    }
}

numbers.forEach {num ->
        if (num == 2) {
            return@forEach // continue와 동일한 역할
        }
    }
```
<br />

여기서 사용한 @run, @forEach는 Label 기능이다.

특정 expression에 라벨 이름을 붙여서 break, continue, return 등을 사용하는 기능이다.

<br />

java에도 있으며 아래와 같이 사용한다.

```java
root: for(int i = 0; i<10; i++) {
    for(int j = 0; j < 10; j++) {
        if(j == 2) {
            break root;
        }
    }
}
```
<br />

위 코드와 동일한 코틀린 코드는 다음과 같다.

```kotlin
root@ for(i in 1..10) {
    for(j in 1..10) {
        if(j == 2) {
            break@root
        }
    }
}
```

<br />

하지만 이렇게 Label을 이용한 Jump 기능은 코드 복잡성을 높일 수 있기 때문에 지양하는 것을 추천한다.

<br />

## takeIf, TakeUnless
- takeIf
    : 주어진 조건을 만족하면 그 값이, 아니면 null이 반환된다.
- takeUnless
    : 주어진 조건을 만족하지 않으면 그 값이, 아니면 null을 반환한다.

첫번째 함수와 아래 두 함수는 완전히 동일한 역할을 한다.

```kotlin
fun getNumberOrNull(): Int? {
    return if(number <= 0) null
        else number
}

fun getNumberOrNull2(): Int? {
    return number.takeIf { number -> number > 0 }
}

fun getNumberOrNull3(): Int? {
    return number.takeUnless { it <= 0 }
}
```

<br />

------
### 참조
- [자바 개발자를 위한 코틀린 입문(Java to Kotlin Starter Guide)](https://www.inflearn.com/course/java-to-kotlin/dashboard)