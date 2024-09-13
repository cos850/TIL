# 코틀린에서 반복문을 다루는 방법

> [!NOTE]
> **소스코드**: 
> [lec06: 코틀린에서 반복문을 다루는 방법](https://github.com/cos850/java-to-kotlin-starter-guide/tree/master/src/main/kotlin/com/lannstark/lec06)

<br />

## for each 문
java의 `:` 대신 Kotlin에서는 `in`을 사용한다.
`in` 뒤에 오는 값은 `Iterable` 이 구현된 타입이라면 모두 들어갈 수 있다.

그 외에는 java와 유사함

```Kotlin
val numbers = listOf(1, 2, 3)
for(num in numbers) {
    println(num)
}
```

<br />

## 정통 for 문
`1..3` 코드는 IntRange 를 만든다.\
IntRange는 IntProgress(등차수열)를 상속한다.
따라서 `1..3` 코드는 시작 값: 1, 끝 값: 3, 공차: 1 인 등차수열인 것.

`downTo` 나 `step` 은 중위 함수이기 때문에, 
`1..5 step 2` 는 `1부터_5까지의_등차수열.step(2)`와 같다


```Kotlin
for (i in 1..3) {
    println(i)
}

for (i in 3 downTo 1) { // 중위 표현 지원
    println(i)
}

for (i in 1..5 step 2) {
    println(i)
}
```

## while
while 문은 java와 동일!

<br />

------
### 참조
- [자바 개발자를 위한 코틀린 입문(Java to Kotlin Starter Guide)](https://www.inflearn.com/course/java-to-kotlin/dashboard)