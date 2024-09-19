# java.lang.ClassNotFoundException: kotlin.reflect.full.KClasses

## 원인
스프링은 Bean 생성 시 내부적으로 Reflection 기능을 사용한다.

Java로 작성된 Bean이 코틀린 클래스를 참조하고 있었는데, 코틀린 클래스는 reflection 기능을 사용하지 못해 발생했다.

## 해결 방법
공식 문서를 확인해보면, 코틀린에서 Reflection 기능을 사용하려면 종속성을 따로 추가하라고 한다.

Reflection 기능을 사용하지 않는 애플리케이션의 런타임 라이브러리 크기를 줄이기 위해서 빼뒀다고 한다.

[kotlin 공식문서 - reflection](https://kotlinlang.org/docs/reflection.html#da792f27)

<br />

### 종속성 추가

- gradle
```gradle
//  kotlin
dependencies {
    implementation(kotlin("reflect"))
}

// groovy
dependencies {
    implementation "org.jetbrains.kotlin:kotlin-reflect:2.0.20"
}
```

<br />

- maven
```xml
<dependencies>
  <dependency>
      <groupId>org.jetbrains.kotlin</groupId>
      <artifactId>kotlin-reflect</artifactId>
  </dependency>
</dependencies>
```