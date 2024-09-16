# 코틀린에서 접근 제어를 다루는 방법

> [!NOTE]
> **소스코드**: 
> [lec11: 코틀린에서 접근 제어를 다루는 방법](https://github.com/cos850/java-to-kotlin-starter-guide/tree/master/src/main/kotlin/com/lannstark/lec11)


<br />

## 코틀린 가시성 제어
- 코틀린에서는 패키지를 통해 접근제어하지 않는다.
- 코틀린의 기본 접근 제어는 public이다.
- 코틀린은 .kt 파일에 변수, 함수, 클래스 여러 개를 바로 만들 수 있다.

<br />

### Java와 Kotlin 접근 제어자 비교

|구분|Java|Kotlin|
|:--:|:--:|:--:|
|public|모든 곳에서 접근 가능|모든 곳에서 접근 가능|
|protected|같은 패키지 내부, 하위 클래스 접근 가능|선언된 클래스, 하위 클래스 접근 가능 <br /> **파일의 최상단에는 사용 불가능**|
|default|같은 패키지 접근 가능|코틀린에는 없음|
|internal|자바에는 없음|같은 모듈에서만 접근 가능|
|private|선언된 클래스 내에서만 접근 가능|선언된 클래스 내에서만 접근 가능|

<br />

> [!NOTE]
> 모듈
> 
> : 한 번에 컴파일되는 Kotlin 코드
> - IDEA Module
> - Maven Project
> - Gradle Source Set
> - Ant Task <Kotlinc> 컴파일 파일의 집합

<br />


> [!WARNING]
> internal은 바이트코드 상 public이 된다. 따라서 Java 코드에서는 Kotlin 모듈의 internal 코드를 가져올 수 있다. 

<br />

> [!WARNING]
> Java 코드에서는 동일한 패키지에 있는 Kotlin의 protected 멤버에 접근할 수 있다.

<br />

------
### 참조
- [자바 개발자를 위한 코틀린 입문(Java to Kotlin Starter Guide)](https://www.inflearn.com/course/java-to-kotlin/dashboard)