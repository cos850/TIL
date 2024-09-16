# 코틀린에서 중첩 클래스를 다루는 방법

> [!NOTE]
> **소스코드**: 
> [lec13: 코틀린에서 중첩 클래스를 다루는 방법](https://github.com/cos850/java-to-kotlin-starter-guide/tree/master/src/main/kotlin/com/lannstark/lec13)


<br />

## 중첩 클래스
> [!TIP]
> 이펙티브 자바에서 일반 내부 클래스는 외부 클래스에 참조로 인해 메모리 누수 등의 성능 문제가 있을 수 있다고 한다.
>
> 그래서 내부 클래스를 만들 땐 static 으로 만들도록 권장하고 있다.

코틀린에서 내부 클래스르 만들면 기본적으로 static 내부 클래스를 만들게 된다.

```kotlin
class House(
    private val address: String,
    private val livingRoom: LivingRoom
) {
    class LivingRoom(
        private var area: Double
    )
}
```

<br />

코틀린에서 외부 클래스와 연결될 수 있는 내부 클래스를 선언하려면 `inner` 키워드를 붙여줘야 한다.

또한, `this@외부클래스` 를 통해 외부 클래스에 접근할 수 있다.

```kotlin
class House(
    private val address: String,
    private val livingRoom: LivingRoom
) {
    inner class LivingRoom(
        private var area: Double
    ) {
        val address: String
            get() = this@House.address
    }
}
```

<br />

> [!NOTE]
> **`const` 키워드**
>
> `const`를 붙이면 컴파일 시에 변수가 할당된다. \
> 그냥 `val MIN_AGE = 1` 이면, 런타임시에 값이 할당된다. \
> 진짜 상수에 붙이는 용도이기 때문에, 기본 타입과 `String` 타입에만 사용할 수 있다.

<br />

이 `companion object`는 하나의 객체로 간주되기 때문에 이름을 붙일 수 있다.

```kotlin
companion object Factory : Log{
    private const val MIN_AGE = 1
    fun newBody(name: String): Person {
        return Person(name, MIN_AGE)
    }

    override fun log() {
        TODO("Not yet implemented")
    }
}
```

> [!TIP]
> `companion object` 에 유틸 함수를 넣어도 되지만, 파일 최상단을 활용하는 것 추천!

<br />

### Java에서 활용하기

위 코틀린 코드를 변환하면 아래와 같다. (Factory 이름 붙이기 전)
```java
public final class Person {
    /* ... */
   private static final int MIN_AGE = 1;
   @NotNull
   public static final Companion Companion = new Companion((DefaultConstructorMarker)null);

   /* ... */

   public static final class Companion {
      @NotNull
      public final Person newBody(@NotNull String name) {
         Intrinsics.checkNotNullParameter(name, "name");
         return new Person(name, 1, (DefaultConstructorMarker)null);
      }

      private Companion() {
      }

      // $FF: synthetic method
      public Companion(DefaultConstructorMarker $constructor_marker) {
         this();
      }
   }
}
```
- `companion object` 내부의 함수는 `Companion` 이라는 `static` 클래스로 만들어진다.
- `MIN_AGE`는 `static` 변수가 된다.

<br />

따라서 Java에서 다음과 같이 사용할 수 있다.
```java
public static void main(String[] args) {
    Person.Companion.newBody("ABC");
}
```

<br />

companion object에 Factory라는 이름을 붙인 경우에는 다음과 같이 사용한다.

```java
public static void main(String[] args) {
    Person.Factory.newBaby("ABC");
}
```

<br />

companion object 내부의 함수를 static method로 선언하고 싶은 경우에는 `@JvmStatic` 을 붙이면 바로 호출할 수 있다.

```Kotlin
companion object {
    @JvmStatic
    fun newBody2(name: String): Person {
        return Person(name, MIN_AGE)
    }
}
```

```java
public static void main(String[] args) {
    Person.newBody2("ABC");
}
```

<br />

## 싱글톤
`object` 키워드를 사용해 싱글톤 클래스를 만든다.

```kotlin
object Singleton {
    var a: Int = 0
}

fun main() {
    println("a = ${Singleton.a}")   // a = 0
    Singleton.a = 10
    println("a = ${Singleton.a}")   // a = 10
}
```

<br />

## 익명 클래스
일회성으로 사용하는 익명 클래스를 코틀린에서도 만들 수 있다.

`object` 키워드를 통해 만든다.

```kotlin
interface Movable {
    fun move()
    fun fly()
}

private fun moveSomething(movable: Movable) {
    movable.move()
    movable.fly()
}

fun main() {
    Person.newBody("ABC")

    println("a = ${Singleton.a}")
    Singleton.a = 10
    println("a = ${Singleton.a}")
    
    moveSomething(object : Movable{
        override fun move() {
            println("움직인다")
        }

        override fun fly() {
            println("날다")
        }
    })

}
```
<br />

------
### 참조
- [자바 개발자를 위한 코틀린 입문(Java to Kotlin Starter Guide)](https://www.inflearn.com/course/java-to-kotlin/dashboard)