# 코틀린에서 클래스를 다루는 방법

> [!NOTE]
> **소스코드**: 
> [lec09: 코틀린에서 클래스를 다루는 방법](https://github.com/cos850/java-to-kotlin-starter-guide/tree/master/src/main/kotlin/com/lannstark/lec09)

<br />

## 클래스 만들기

1. 코틀린에서 생성자는 클래스명 옆에 `contructor` 키워드를 사용해 만든다.
2. `constructor` 키워드는 생략 가능하다.
3. 클래스의 필드를 선언하면, getter와 setter는 자동으로 만들어준다.
4. 필드 선언을 생성자에서도 할 수 있다.
5. body에 아무런 내용이 없는 경우 생략 가능하다.


### 코틀린 클래스 예시
먼저 기본적인 클래스는 다음과 같은 형태를 가진다.

```kotlin
class Person constructor(name: String, age: Int) {
    val name: String = name
    var age: Int = age

    // getter / setter 자동 생성
}
```
<br />

위의 클래스는 아래와 같이 간결하게 표현할 수 있다.

```kotlin
class Person (val name: String, var age: Int)
```

<br />

### 프로퍼티
코틀린은 클래스의 필드에 바로 접근해서 getter와 setter를 호출한다.

```kotlin
fun main() {
    val person = Person("철수", 5)
    person.age = 10
    println("${person.name}의 나이는 ${person.age}이다.")
}
```

> [!NOTE]
> Java 클래스를 사용할 때도 `person.age` 처럼 바로 필드에 접근해 getter / setter를 호출할 수 있다.


<br />

## 코틀린 클래스 초기화

<br />

### 1. init 블록
코틀린에서는 `init`이라는 특별한 블록이 있다. \
클래스를 초기화하는 시점에 한 번 호출되어 실행한다. \
이 init 블럭을 여러 개 넣어도 전부 초기화 시 호출된다.

```kotlin
class Person (val name: String, var age: Int) {
    init {
        if(age <= 0) {
            throw IllegalArgumentException()
        }
    }
}
```
<br />

### 2. constructor
생성자는 두 가지 방식이 있다.

- **primary constructor (주생성자)** \
    : 위에서 본 `Person` 클래스처럼 class 정의할 때 클래스 명 뒤에 붙는 생성자.
    - 반드시 존재해야 한다.
    - 파라메터가 하나도 없으면 기본 생성자가 만들어지니까 생략 가능!

- **secondary constructor (부생성자)** \
    : 클래스 내부에 정의하는 생성자 함수. 
    - 여러 개 정의 가능!
    - 반드시 `this` 키워드를 통해 주 생성자 호출해야 한다.
    - body를 가질 수 있다.

```kotlin
// primary constructor
class Person (val name: String, var age: Int) {

    // secondary constructor
    constructor(name: String) : this(name, 1) {
        println("부생성자 1 - name만 인수로")
    }

    constructor(): this("홍길동") {
        println("부생성자 2 - 아무것도 받지 않음")
    }
}
```
<br />

> [!TIP]
> 코틀린에서는 부생성자보다 default parameter를 권장한다.
> Converting과 같은 경우, 부생성자를 사용하기보다 정적 팩토리 메서드 패턴을 추천한다.
> 
> - 예시 \
> `class Person(val name: String, var age: Int = 1)`

<br />

### 호출 순서
이제 호출되는 순서를 알아보자.

초기화 순서를 알기 위해 init 블록과 여러 개의 생성자를 만든다.

```kotlin
class Person (val name: String, var age: Int) {
    init {
        println("초기화 블록")
        if(age <= 0) {
            throw IllegalArgumentException()
        }
    }

    constructor(name: String) : this(name, 1) {
        println("부생성자 1 - name만 인수로")
    }

    constructor(): this("홍길동") {
        println("부생성자 2 - 아무것도 받지 않음")
    }
}
```
<br />

main 함수에서 Person 생성해보자.
```kotlin
fun main() {
    val person = Person()
}
```
<br />

역순으로 출력되는 것을 알 수 있다.

```bash
# 출력 결과
초기화 블록
부생성자 1 - name만 인수로
부생성자 2 - 아무것도 받지 않음
```

<br />

## custom getter / setter
필드를 선언하면 자동으로 만들어주는 getter/setter를 변경할 수 있다.

```kotlin
class Person (name: String, var age: Int) {

    var name: String = name
        set(value) {
            field = value.uppercase()
        }

    val isAdult: Boolean
        get() = this.age >= 20
}
```
<br />

> [!WARNING]
> **backing field** \
> custom getter / setter에서 자기 자신을 접근할 떄 `field` 키워드를 사용하지 않으면, 무한 루프에 빠질 수 있으므로 주의해야 한다.

위 코틀린 `Person` 클래스 코드를 디컴파일러를 통해 자바로 변환해보면 아래와 같다.

```java
public final class Person {
   @NotNull
   private String name;
   private int age;

   @NotNull
   public final String getName() {
      return this.name;
   }

   public final void setName(@NotNull String value) {
      Intrinsics.checkNotNullParameter(value, "value");
      String var10001 = value.toUpperCase(Locale.ROOT);
      Intrinsics.checkNotNullExpressionValue(var10001, "(this as java.lang.Strin….toUpperCase(Locale.ROOT)");
      this.name = var10001;
   }

   public final boolean isAdult() {
      return this.age >= 20;
   }

   public final int getAge() {
      return this.age;
   }

   public final void setAge(int var1) {
      this.age = var1;
   }

   public Person(@NotNull String name, int age) {
      Intrinsics.checkNotNullParameter(name, "name");
      super();
      this.age = age;
      this.name = name;
   }
}
```
<br />

- `age` 필드는 getter/setter 자동 생성
- `isAdult` 함수로 변환
- `name`의 stter는 커스텀한 내용 반영



<br />

------
### 참조
- [자바 개발자를 위한 코틀린 입문(Java to Kotlin Starter Guide)](https://www.inflearn.com/course/java-to-kotlin/dashboard)