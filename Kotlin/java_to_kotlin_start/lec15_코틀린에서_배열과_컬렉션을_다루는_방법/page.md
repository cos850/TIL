# 코틀린에서 배열과 컬렉션을 다루는 방법

> [!NOTE]
> **소스코드**: 
> [lec15: 코틀린에서 배열과 컬렉션을 다루는 방법](https://github.com/cos850/java-to-kotlin-starter-guide/tree/master/src/main/kotlin/com/lannstark/lec15)


<br />


## 배열 
코틀린에서는 배열을 편리하게 사용할 수 있는 기능을 제공한다.

```kotlin
val array = arrayOf(100, 200)

for (i in array.indices) {
    println("${i}, ${array[i]}")
}

for ((i, value) in array.withIndex()) {
    println("${i}, ${value}")
}

array.plus(300)
```

- `arrayOf()` : 배열을 편리하게 생성할 수 있음
- `array.indices` : IntRange(0, lastIndex) 생성해서 전달
- `array.withIndex()`: `index, value` 를 가지는 클래스의 배열 반환
- `array.plus()`: 배열에 간단하게 값을 추가할 수 있도록 도와줌

<br />

## Collection
컬렉션은 불변과 가변으로 나뉘어져 있다.

- **불변**: 기본 컬렉션들, element를 추가/삭제할 수 없다. (Collection, List, Map, Set)
- **가변**: Mutable 컬렉션, element 추가/삭제 가능 (Mutable Collection, Mutable List, Mutable Map ..)

불변 컬렉션은 읽기 전용으로 사용되며,
컬렉션에 element를 추가하거나 삭제하려면 Mutable Collection을 사용해야 한다.

<br />

### 리스트
- **불변 리스트**
```kotlin
val emptyList = emptyList<Int>()
val numbers = listOf(100, 200)
printNumbers(emptyList())

for (number in numbers) {
    println(number)
}

for ((idx, value) in numbers.withIndex()) {
    println("${idx}. ${value}")
}
```

<br />

- **가변 리스트**
```kotlin
val mutableList = mutableListOf(100, 200)
mutableList.add(300)
```

<br />

> [!TIP]
> 불변 컬렉션의 element를 추가/삭제할 순 없지만,\
> 객체인 element의 **내부 필드의 값은 변경**할 수 있다.
> 
> **ex) list에 담긴 Person 객체의 이름 변경 예시**
> ```kotlin
> class Person(var name: String)
> val list = listOf(Person("철수"))
> //list.add(Person("짱구"))    // 불가능
> //list[3] = Person("짱구")    // 불가능
>                               
> list[0].name = "맹구"         // 가능!            
> ```

<br />

### Set
리스트에서 사용할 수 있는 기능은 대부분 사용 가능

`setOf()`를 통해 생성할 경우 `LinkedHashSet` 사용!

```kotlin
val numberSet = setOf(100, 200)
val mutableSet = mutableSetOf(100, 200)
```

<br />

### Map
Map도 마찬가지로 mutableMap, Map 으로 나누어 사용할 수 있다.

특이한 점은 Map도 []를 통해 key에 접근할 수 있다는 것이다.

`mapOf()`는 `Pair`를 인수로 받는 함수이기 때문에, `to()`를 활용해 초기화할 수 있다.

```kotlin
val map = emptyMap<Int, String>()

val intKeyMap = mutableMapOf<Int, String>()
intKeyMap.put(1, "철수")
intKeyMap[1] = "철수"

val stringKeyMap = mutableMapOf("철수" to 1, "유리" to 2, "맹구".to(3))
stringKeyMap["철수"] = 3
```
<br />

또한 코틀린에서 만들어준 `entries`를 통해 key와 value를 쉽게 꺼내 쓸 수 있다.

```kotlin
for(key in intKeyMap.keys) {
    println(key)
    println(intKeyMap[key])
}

for((key, value) in intKeyMap.entries) {
    println("${key}, ${value}")
}
```

<br />

## Collection의 Null 가능성
- `List<Int>` : List non-nullable, Element non-nullable
- `List<Int?>` : List non-nullable, Element nullable
- `List<Int>?` : List nullable, Element non-nullable
- `List<Int?>?` : List nullable, Element nullable

**=> `?` 위치에 따라 null 가능성의 의미가 달라진다.**

<br />

## Java와 함께 사용 시 유의점
### 읽기 전용 컬렉션 / 가변 컬렉션
Java는 읽기 전용 컬렉션 / 가변 컬렉션을 구분하지 않는다.

따라서 Kotlin의 불변 리스트를 Java가 가져오면 element를 추가할 수 있다!!

<br />

### nullable 컬렉션
Java는 nullable 과 non-nullable 컬렉션을 구분하지 않는다.

따라서 코틀린의 non-nullable 컬렉션을 Java에서 가져오면 null을 리스트에 넣을 수 있다!

> [!IMPORTANT]
> 코틀린의 컬렉션이 Java에게 호출될 수 있다면, 방어 로직 등을 추가해줘야 한다!
> 
> 아니면, `Collections.unmodifableXXX()`를 활용해 변경을 막자.

<br />

> [!IMPORTANT]
> 또한, Java에서 가져온 리스트도 플랫폼 타입임을 감안해서 가져오는 지점을 wrapping 하자

------
### 참조
- [자바 개발자를 위한 코틀린 입문(Java to Kotlin Starter Guide)](https://www.inflearn.com/course/java-to-kotlin/dashboard)