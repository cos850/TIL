# 코틀린에서 제어문을 다루는 방법

> [!NOTE]
> **소스코드**: 
> [lec04: 코틀린에서 제어문을 다루는 방법](https://github.com/cos850/java-to-kotlin-starter-guide/tree/master/src/main/kotlin/com/lannstark/lec05)

## Kotlin에서 `if`문은 Expression이다!
if-else는 \ 
Java에서는 Statement 지만 \
Kotlin에서는 Expression으로 취급된다.

따라서, Kotlin에는 3항 연산자가 필요하지 않기 때문에 없다.

<br />

> [!NOTE]
> **Statement / Expression**
> - Statement: 프로그램의 문장
> - Expression: Statement 중 하나의 값으로 도출되는 문장

<br />

### Kotlin if-else 예시
```kotlin
fun getPassOrFail(score: Int): String {
    return if(score >= 50) {
        return "Pass"
    } else {
        "Fail"  // return 생략 가능
    }
}
```

<br />

## Kotlin when
Kotlin의 `when`은 Java의 `switch` 문과 비슷한 역할을 한다.

```kotlin
fun getGradeWithSwitch01(score: Int) {
    when(score / 10) {
        9 -> "A"
        8 -> "B"
        7 -> "C"
        else -> "D"
    }
}

fun getGradeWithSwitch02(score: Int): String {
    return when(score) {
        in 90..100 -> "A"
        in 80.. 89 -> "B"
        in 70..79 -> "C"
        else -> "D"
    }
}

fun startWithA(obj: Any): Boolean {
    return when(obj) {
        is String -> obj.startsWith("A")    // 스마트캐스팅 
        else -> false
    } 
}

fun judgeNumber2(number: Int) {
    when {
        number == 0 -> println("주어진 숫자는 0입니다.")
        number % 2 == 0 -> println("주어진 숫자는 짝수입니다.")
        else -> println("주어진 숫자는 홀수입니다.")
    }
}
```

------
### 참조
- [자바 개발자를 위한 코틀린 입문(Java to Kotlin Starter Guide)](https://www.inflearn.com/course/java-to-kotlin/dashboard)