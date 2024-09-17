# 코틀린에서 다양한 함수를 다루는 방법

> [!NOTE]
> **소스코드**: 
> [lec16: 코틀린에서 다양한 함수를 다루는 방법](https://github.com/cos850/java-to-kotlin-starter-guide/tree/master/src/main/kotlin/com/lannstark/lec16)


<br />


## 확장 함수
: 객체의 멤버 함수처럼 사용할 수 있지만 함수는 해당 객체 밖에 정의하는 것

### 문법
```kotlin
fun [확장클래스].[함수명](): [반환타입] {
    /** 함수 본문 */ 
}
```

<br />

### 예시 - String을 확장하는 lastChar 함수 정의
```kotlin
fun main() {
    /**
     * 확장 함수
     */
    println("ABC".lastChar())
}

fun String.lastChar(): Char = this[this.lastIndex]
```
<br />

- 함수 안에서는 `this` 를 통해 인스턴스에 접근이 가능하다.
- 이 때 `this`를 "수신객체"라고 부른다.
- 위 예시의 `String`처럼 확장하는 클래스를 "수신객체 타입"이라고 부른다.
- 사용할 때는, 확장한 객체의 멤버 변수처럼 `.` 을 통해 호출할 수 있다.

<br />

> [!NOTE]
> **수신객체의 멤버변수 접근제어**
> 
> `this`를 통해 해당 객체의 멤버에 접근할 수 있는데, 만약 private나 protected 멤버에 접근할 수 있게 되면 캡슐화가 깨질 수 있으므로 호출할 수 없게 되어있다!

<br />

### 확장 함수의 중복 정의
확장 함수와 멤버 함수의 함수명과 파라메터가 동일하다면 **멤버 함수가 우선적으로 호출**된다!

따라서 추후 다른 기능의 똑같은 멤버 함수가 생기면 원하는 대로 동작하지 않을 수 있으므로 주의하자.

```kotlin
fun main() {
    val person = Person("철수", 5)
    println(person.nextYearAge())   // 멤버 함수 호출
}

class Person(val name: String, var age: Int) {
    fun nextYearAge(): Int {
        println("member function")
        return this.age + 1
    }
}

fun Person.nextYearAge(): Int {
    println("extension function")
    return this.age + 1
}
```

<br />

또한, 부모 클래스와 자식 클래스에 동일한 확장 함수를 정의한다면 어떤 함수가 호출되는지 알아보자.

```kotlin
open class Train()
class Srt() : Train()

fun Train.isRunning() = true
fun Srt.isRunning() = false

fun main() {
    val train = Train()
    train.isRunning()       // train의 멤버함수 호출

    val srtTrain: Train = Srt()
    srtTrain.isRunning()    // train의 멤버함수 호출

    val srt = Srt()
    srt.isRunning()         // srt의 확장함수 호출
}
```

> [!NOTE]
> 확장 함수를 호출할 때는, 명시한 정적 타입의 확장 함수가 호출되는 것을 알 수 있다. 해당 인스턴스가 실제로 어떤 타입인지 중요하지 않다.


### 확장 프로퍼티
custom getter를 통해 확장 프로퍼티도 만들 수 있다.

```kotlin
val String.lastChar: Char
    get() = this[this.lastIndex]

fun main() {
    println("ABCD".lastChar)
}
```


## infix 함수
인수가 하나인 함수에 대해, 함수를 호출하는 방법을 다르게 할 수 있도록 지원하는 것!

함수에 `infix` 키워드를 붙여 정의한다.

### infix 함수 예시 - 확장 함수
```kotlin
fun main() {
    var i = 1
    
    println(i.add(1))
    println(i.infixAdd(1))
    println(i infixAdd 1)   // infix 방식 호출
}

fun Int.add(other: Int): Int {
    return this + other
}

infix fun Int.infixAdd(other: Int): Int {
    return this + other
}
```

<br />


## inline 함수
함수가 호출하는 것이 아니라, 함수의 로직이 붙여넣기 되는 것

`inline` 키워드를 붙여 정의한다.

주로 함수 호출의 오버헤드를 줄이기 위해 사용되며, 성능 측정과 함께 신중하게 사용해야 한다.

```kotlin
fun main() {
 3.inlineAdd(4)   
}

inline fun Int.inlineAdd(other: Int): Int {
    return this + other
}
```

<br />


## 지역 함수
함수안에 함수를 선언할 수 있는 것

depth가 깊어지게 되고, 코드가 깔끔하지 않기 때문에 잘 사용되지 않는다.

```kotlin
fun createPerson(firstName: String, lastName: String): Person {
    fun validateName(name: String, fieldName: String) {
        if (name.isEmpty()) {
            throw IllegalArgumentException("${fieldName}은 비어있을 수 없음!!!")
        }
    }

    validateName(firstName, "firstName")
    validateName(lastName, "lastName")

    return Person(lastName + firstName, 1)
}
```


<br />

------
### 참조
- [자바 개발자를 위한 코틀린 입문(Java to Kotlin Starter Guide)](https://www.inflearn.com/course/java-to-kotlin/dashboard)