# 코틀린의 Scope Function

> [!NOTE]
> **소스코드**: 
> [lec20: 코틀린의 Scope Function](https://github.com/cos850/java-to-kotlin-starter-guide/tree/master/src/main/kotlin/com/lannstark/lec20)


<br />

 
## scope function
: 일시적인 영역을 형성하는 함수

람다를 활용해 일시적인 영역을 만들고, 코드를 더 간결하게 하거나, method chaning에 활용하는 함수를 scope function이라고 한다.

### 종류
|구분|함수를 인자로 받음|확장 함수를 인자로 받음|
|--|--|--|
|람다의 결과를 반환|let|run|
|객체 그 자체를 반환|also|apply|

**+** with

<br />

### let, run, also, apply 예시
```kotlin
val person = Person()

val personAge1 = person.let { it.age }
val personAge2 = person.run { this.age }

val person1 = person.also { it.age }
val person2 = person.apply { this.age }
```

- let과 also는 함수를 인자로 받기 떄문에, 내부에서 it이나 p -> 와 같이 이름을 지정할 수 있다.
- run과 apply는 person에 대한 확장함수를 인자로 받고 있기 때문에 내부적으로 this를 통해 접근할 수 있다.

<br />

### with 예시
with는 파라메터와 람다를 인수를 받는다.
따라서 내부 코드에서 this를 통해 접근할 수 있다.

```kotlin
with(person) {
    println(this.name)
    println(age)
}
```

<br />

### 리펙터링 예시
예를 들어, 아래와 같은 코드가 있다고 하자.
```kotlin
fun printPerson(person: Person?) {
    if(person != null) {
        println(person.name)
        println(person.age)
    }
}
```

이 코드를 let 이라는 함수를 통해 아래와 같이 리펙터링할 수 있다.
```kotlin
fun printPerson(person: Person?) {
    person?.let {
        println(it.name)
        println(it.age)
    }
}
```

<br />

> [!NOTE]
> scope function을 사용하면 반드시 가독성이 좋아지지 않고, 오히려 가독성과 유지보수성이 떨어지게 될 수도 있다.
>
> 따라서 적재적소에 잘 활용할 수 있도록 고민하며 적용하자.

<br />

------
### 참조
- [자바 개발자를 위한 코틀린 입문(Java to Kotlin Starter Guide)](https://www.inflearn.com/course/java-to-kotlin/dashboard)