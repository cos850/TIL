# 코틀린에서 타입을 다루는 방법

> [!NOTE]
> **소스 코드:** 
> [github - lec03: Kotlin에서 type을 다루는 방법](https://github.com/cos850/java-to-kotlin-starter-guide/tree/master/src/main/kotlin/com/lannstark/lec03)

<br />

## 1. 기본 타입
코틀린에서는 선언된 기본 값을 보고 타입을 추론한다.
 
```kotlin
val number1 = 1    // Int
val number2 = 1L   // Long
```

```kotlin
val number1 = 3.0f  // Float
val number2 = 3.0   // Double
```

<br />

> [!NOTE]
> Java에서는 기본 타입간의 변환은 **암시적**으로 이루어질 수 있음
> Kotlin에서는 기본 타입간의 변환이 **명시적**으로 이루어져야 함

**예시)**
- java에서는 int 타입이 long 타입으로 암시적 변환

    ```java
    int number1 = 4;
    long number2 = number1; // int -> long

    System.out.println(number1 + number2)
    ```
    <br />

    - kotlin에서는 `toLong()`을 통해 변환
    ```kotlin
    val number1 = 4;
    val number2 = number1;          // type mismatch
    val number3 = number1.toLong()  // 명시적 변환

    println(number1 + number2)
    ```

<br />

## 2. 타입 캐스팅 (`is`, `as`)

- `is`: java의 `instanceof` 와 동일한 역할
- `as`: java의 강제 형변환과 동일한 역할

```kotlin
var obj: Any = Person("철수", 23)
if(obj is Person) {
    val person = obj as Person
    println(person.age)
}

// 스마트 캐스트
if(obj is Person) {
    println(obj.age)
}
```

<br />

- `!is`: `is`가 아니면
- `as?`: 앞의 요소가 null이면 null, \
    아니면 형변환, \
    null도 아니고 타입도 다르면 null (오류X)

```kotlin
var obj: Any = Person("철수", 23)
if(obj !is Person) {    // obj이 Person이 아니면
    // ...
}

var obj2: Any? = Person("철수", 23)
val person = obj2 as? Person
```


<br />

## 3. Kotlin 전용 타입

### Any
- java의 Object 역할, 모든 Primitive Type의 최상위 타입
- non-null
<br />

### Unit
- java의 void와 동일한 역할
- void와 다르게, Unit은 타입으로 사용가능
<br />

### Nothing
- 함수가 정상적으로 끝나지 않음을 표현
- 무조건 예외를 반환하는 함수나 무한 루프에 사용

<br />

## String 활용

### String interpolation

```kotlin
val person2 = Person("철수", 23)
val personDesc = "이름은 ${person2.name}, 나이는 ${person2.age}"
```
<br />

### String indexing
```kotlin
val str = "ABCD"
val ch = str[2]
```
<br />

### `"""`
```kotlin
"""
    Hello
    ${person2.name}
""".trimIndent()
```

<br />

------
### 참조
- [자바 개발자를 위한 코틀린 입문(Java to Kotlin Starter Guide)](https://www.inflearn.com/course/java-to-kotlin/dashboard)
