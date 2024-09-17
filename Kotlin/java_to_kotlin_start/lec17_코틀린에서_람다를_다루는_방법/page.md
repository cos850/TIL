# 코틀린에서 람다를 다루는 방법

> [!NOTE]
> **소스코드**: 
> [lec17: 코틀린에서 람다를 다루는 방법](https://github.com/cos850/java-to-kotlin-starter-guide/tree/master/src/main/kotlin/com/lannstark/lec17)


<br />

 
## 코틀린에서의 람다
Java에서는 람다를 사용해 함수를 인자로 주는 것처럼 보여도, 미리 선언된 함수형 인터페이스를 인자로 받는 것이다. 

이처럼 Java에서는 함수를 변수에 할당하거나, 인자로 받을 수 없다.

하지만, 코틀린에서는 함수가 그 자체로 값이 될 수 있다.

변수에 할당할 수도, 파라미터로 넘길 수도 있다.

### 변수에 익명함수 할당하기
```kotlin
// 정의
val isApple: (Fruit) -> Boolean = fun(fruit: Fruit): Boolean {
    return fruit.name == "사과"
}
val isApple2 = {fruit: Fruit -> fruit.name == "사과"}

// 호출
isApple(fruits[0])
isApple.invoke(fruits[0])
```
<br />

### 함수 파라메터 활용
함수를 파라메터로 받아서 처리해보자.

아래와 같이 함수를 파라메터로 받는 함수를 정의해서 호출할 수 있다.

```kotlin
fun main() {
    filterFruits(fruits, isApple)   // 함수 타입의 변수 전달
    filterFruits(fruits, { fruit: Fruit -> fruit.name == "사과" })  // 함수를 파라메터에서 바로 정의
}

private fun filterFruits(
    fruits: List<Fruit>, filter: (Fruit) -> Boolean
): List<Fruit> {
    val result = mutableListOf<Fruit>()
    for(fruit in fruits) {
        if(filter(fruit)){
            result.add(fruit)
        }
    }

    return result
}
```

<br />
위처럼 함수를 파라메터에서 바로 정의할 때 함수가 마지막 인자라면 아래와 같이 함수 정의를 뺄 수 있다.

```kotlin
filterFruits(fruits) { 
    fruit: Fruit -> fruit.name == "사과"
}
```
<br />

또한, 파라미터의 타입을 통해 추론이 가능하므로 타입 정의도 생략이 가능하다.
```kotlin
filterFruits(fruits) {
    fruit -> fruit.name == "사과"
}
```
<br />

또한, 파라미터가 1개인 경우, `it` 키워드를 통해 직접 참조할 수 있다.

```kotlin
filterFruits(fruits) {
    it.name == "사과"
}
```

> [!NOTE]
> 람다는 여러 줄 작성할 수 있으며, return을 명시하지 않아도 마지막 줄의 결과가 람다의 반환 값이 된다.


<br />

## Closure
Java의 경우 람다에서 사용하는 외부 변수는 final이 아닐 경우 사용하지 못한다.

하지만 코틀린에서는 final이 아닌 변수도 사용할 수 있다.

```kotlin
var target = "바나나"
target = "딸기"
filterFruits(fruits) { it.name == target }
```

> [!NOTE]
> 코틀린은 람다가 시작하는 지점에 참조하는 변수를 모두 포획해서 기억하고 있다.
>
> 이러한 데이터 구조를 Closure라고 부른다.


<br />

## try-with-resource와 use
저번에 try-with-resource 대신 use를 사용할 수 있다는 것을 배웠었다.

use의 코드를 하나씩 살펴보면서 어떻게 작동하는지 이해해보자.

### use 코드
```kotlin
public inline fun <T : Closeable?, R> T.use(block: (T) -> R): R { 
    /* ... */ 
}
```

- Closeable 구현체(T)에 대한 확장 함수이다.
- 해당 함수는 inline 함수이다. (함수 본문이 복사되는 함수)
- use는 block이라는 이름의 함수를 파라미터로 받고 있다. (이 block은 T 타입의 파라메터를 받아서, R 타입의 파라메터를 반환한다.)


<br />

### use를 사용한 코드

```kotlin
BufferedReader(FileReader(path)).use { 
    reader -> println(reader.readLine())
}
```

- BufferedReader는 Closeable 구현체이기 때문에 use 확장 함수를 사용할 수 있다.
- use의 파라미터인 block 함수를 바로 정의해서 전달했다.
- 위 코드에서 block 함수는 BufferedReader 타입을 받아서 Unit 타입을 반환한다.

<br />

------
### 참조
- [자바 개발자를 위한 코틀린 입문(Java to Kotlin Starter Guide)](https://www.inflearn.com/course/java-to-kotlin/dashboard)