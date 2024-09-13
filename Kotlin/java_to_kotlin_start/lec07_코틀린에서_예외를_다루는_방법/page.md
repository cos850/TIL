# 코틀린에서 예외를 다루는 방법

> [!NOTE]
> **소스코드**: 
> [lec07: 코틀린에서 예외를 다루는 방법](https://github.com/cos850/java-to-kotlin-starter-guide/tree/master/src/main/kotlin/com/lannstark/lec07)

<br />

## try-catch-finally
try-catch 또한 Expression으로 판단되어 return과 함께 사용할 수 있다. 

```
fun parseIntOrThrow2(str: String): Int? {
    return try {
        str.toInt()
    } catch (e: NumberFormatException) {
        null
    }
}
```

<br />

## Checked Exception / Unchecked Exception
java에서는 checked exception은 throws로 명시해줘야 한다.
하지만 코틀린에서는 모두 unchecked exception 간주한다.

따라서, `thorws` 를 붙여주지 않아도 오류가 발생하지 않는다.

```kotlin
// IOException이 발생할 수 있지만, 오류가 발생하지 않음
fun readFile() {
    val currentFile = File(".")
    val file = File(currentFile.absolutePath + "/a.txt")
    val reader = BufferedReader(FileReader(file))
    println(reader.readLine())
    reader.close()
}
```

<br />

## try-with-resource
kotlin에서는 java의 try-with-resource가 사라지고, `use`를 사용한다!

```kotlin
fun readFile(path: String) {
    BufferedReader(FileReader(path)).use { reader ->
        println(reader.readLine())
    }
}
```

<br />

------
### 참조
- [자바 개발자를 위한 코틀린 입문(Java to Kotlin Starter Guide)](https://www.inflearn.com/course/java-to-kotlin/dashboard)