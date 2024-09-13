# Kotlin에서 null을 다루는 방법

- kotlin에서는 nullable 변수의 내부로 접근하면 컴파일 오류가 발생한다.
- 하지만, nullable 객체에 접근하기 전에 위에서 null 검사를 해주면, 컴파일러가 null이 아니라고 간주해서 객체 내부의 요소에 접근할 수 있게 된다.

<br />

## safe call (`?.`)
null이 아니면 실행하고, null이면 실행하지 않는다.
```kotlin
var str1: String? = "ABC"
print(str1.length)  // 오류!
println(str1?.length) // 가능
```
<br />

`str1`이 null일 경우, null을 반환한다.
```kotlin
str1 = null
println(str1?.length)   // 결과: null
```


<br />

## elvis 연산자 (`?:`)
앞의 연산 결과가 null이면 뒤의 값을 사용

```kotlin
val str2: String? = "BAC"
println(str2?.length ?: 0)    // 결과: 3
```


<br />

## non-null 단정자 (`!!.`)
nullable 값이, 절대 null이 아닐 경우, \
`!!.`를 통해 컴파일러에게 null이 절대 아닐 것을 단언할 수 있음 \
하지만 null이 들어오게 된다면 `NullPointerException` 발생

```kotlin
fun startsWithNotNull01(str: String?): Boolean {
    return str!!.startsWith("A")
}
```


<br />

## 플랫폼 타입
java의 클래스를 kotlin에서 사용할 수 있는데, 해당 클래스의 필드가 null인지 아닌지 모르는 것을 플랫폼 타입이라고 한다.
코틀린의 non-null 타입과 일단 호환되도록 지원한다.

예를들어, 아래와 같이 `@Nullable`을 통해 해당 필드가 null이 가능하다고 명시되어 있다면

```java
// java Person 클래스
import org.jetbrains.annotations.Nullable;

public class Person {

  private final String name;

  public Person(String name) {
    this.name = name;
  }

  @Nullable
  public String getName() {
    return name;
  }

}
```
<br />

코틀린은 이를 해석해서 아래 코드를 실행할 수 없게 한다.

```kotlin
val person = Person("ABC")
startsWithNotNull(person.name) // @Nullable로 인한 오류 !

fun startsWithNotNull(str: String): Boolean {
    return str.startsWith("A")
}
```

<br />

그런데 Person 클래스에 `@Nullable` 이 없다면, 코틀린은 이 값이 nullable인지 non-null 인지 알 수 없다. (**=> 플랫폼 타입**)

> [!WARNING] 
> 코틀린에서는 non-null 타입과 호환이 가능하도록 지원한다.
> 컴파일 오류가 나지 않기 때문에, Runtime에서 Exception이 발생할 수 있다!



<br />

## java 코드를 kotlin으로 변환하기

### 1번 함수
- java 코드
```java
public boolean startsWithA1(String str) {
    if (str == null) {
        throw new IllegalArgumentException("null이 들어왔습니다");
    }
    return str.startsWith("A");
}
```

<br />

- 코틀린으로 변환 
```kotlin
fun startsWithA1(str: String?): Boolean {
    if (str == null) {
        throw IllegalArgumentException("null이 들어왔습니다.")
    }
    return str.startsWith("A")
}
```

<br />

- 더 코틀린스럽게
```kotlin
fun startsWithA1Kt(str: String?): Boolean {
    return str?.startsWith("A")
        ?: throw IllegalArgumentException("null이 들어왔습니다.")
}
```

<br />

### 2번 함수
- java 코드
```java
public Boolean startsWithA2(String str) {
    if (str == null) {
        return null;
    }
    return str.startsWith("A");
}
```

<br />

- 코틀린으로 변환
```kotlin
fun startsWithA2(str: String?): Boolean? {
    if(str == null) {
        return null
    }
    return str.startsWith("A")
}
```

<br />

- 더 코틀린스럽게
```kotlin
fun startsWithA2Kt(str: String?): Boolean? {
    return str?.startsWith("A") // safe call 은 str이 null이면 null 반환
}
```


<br />

### 3번 함수
- java 코드
```java
public boolean startsWithA3(String str) {
    if (str == null) {
        return false;
    }
    return str.startsWith("A");
}
```

<br />

- 코틀린으로 변환
```kotlin
fun startsWithA3(str: String?): Boolean {
    if(str == null) {
        return false
    }
    return str.startsWith("A")
}
```

<br />

- 더 코틀린스럽게
```kotlin
fun startsWithA3Kt(str: String?): Boolean {
    return str?.startsWith("A") ?: false
}
```
