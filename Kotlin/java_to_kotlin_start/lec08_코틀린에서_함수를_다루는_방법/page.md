# 코틀린에서 함수를 다루는 방법

> [!NOTE]
> **소스코드**: 
> [lec08: 코틀린에서 함수를 다루는 방법](https://github.com/cos850/java-to-kotlin-starter-guide/tree/master/src/main/kotlin/com/lannstark/lec08)

<br />

## 함수 선언
1. 반환 타입이 `Unit` 이면 생략 가능, 그 외 명시적 지정
2. 함수 내부에서 바로 `return [Expression]` 인 경우, {} 제거 후 `=` 를 사용해 선언 가능
3. `=` 로 함수를 표현한 경우, 반환 값 생략 가능

위의 규칙을 따를 경우, 아래와 같이 함수를 축약해서 선언할 수 있다.

### 함수 선언 축약 예시
- 기존 kotlin 함수
```kotlin
fun max(a: Int, b: Int): Int {
    return if (a > b) {
        a
    } else {
        b
    }
}
```

<br />

- 위 절차에 따라 축약하여 선언
```kotlin
fun max(a: Int, b: Int) = if (a > b) a else b
```

<br />


> [!NOTE]
> 함수는 파일 최상단, 클래스 내부, 한 파일에 여러 함수가 있을 수 있다!

<br />

## default parameter
java의 오버로딩을 통해 여러 번 선언하던 함수를 kotlin에서는 default paramter를 통해 하나의 함수로 만들 수 있다.

 ```kotlin
// 두 함수는 동일하게 동작!
repeat("Hello World 1", 3, true)
repeat("Hello World 2")

 fun repeat(
    str: String, 
    num: Int = 3, 
    useNewLine: Boolean = true
) {
    for (i in 1..num) {
        if(useNewLine) {
            println(str)
        } else {
            print(str)
        }
    }
}
 ```


<br />

## named argument
위 `repeat()` 함수에서 반복 개수는 기본 값을 사용하고, 개행 여부만 전달하고 싶다면 named argument를 사용할 수 있다. 

```kotlin
repeat("Hello World 3", useNewLine = false)
```

<br />

> [!NOTE]
> named argument 기능을 통해 builder를 구현하지 않고도 builder의 장점을 가질 수 있다!!
> 
> 참고) 코틀린에서 Java 함수를 사용할 때는 해당 기능을 사용할 수 없다.


<br />


## 가변인자
java의 가변 인자를 코틀린에서 구현하려면 `...` 대신 `vararg` 키워드를 사용한다.

```kotlin
fun printAllKt(vararg strings: String) {
    for(str in strings) {
        println(str)
    }
}
```

해당 함수를 호출할 때, 배열을 넘겨줄 경우 spread 연산자 (`*`) 사용해야 한다.

```kotlin
printAllKt("A", "B", "C")

val array = arrayOf("A", "B", "C")
printAllKt(*array)  // spread 연산자 (*) 사용
```



<br />

------
### 참조
- [자바 개발자를 위한 코틀린 입문(Java to Kotlin Starter Guide)](https://www.inflearn.com/course/java-to-kotlin/dashboard)