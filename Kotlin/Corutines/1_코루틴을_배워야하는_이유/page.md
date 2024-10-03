# 코루틴을 배워야 하는 이유

RxJava와 달리, 코루틴을 도입하면 기존 코드를 크게 변경하지 않고도 가능하다.

백엔드의 대부분의 환경에서는 단지 `suspend` 제어자를 추가하는 것으로 충분하다.

수백명의 사용자가 응답을 기다릴 때마다 블로킹하고 있다면 엄청난 비용이 들어가지만, 코루틴을 사용하면 스레드와 비교되지 않을 정도로 저렴해 인지할 수 없을 정도다.

```kotlin
fun main() {
    val start:Long = System.currentTimeMillis()
    repeat(20) {
        thread {
            Thread.sleep(2000L)
            print(".")
        }
    }
    println(System.currentTimeMillis() - start)
}
```

```kotlin
fun main() = runBlocking {
    repeat(20) {
        launch {
            delay(2000L)
            print(".")
        }
    }
}
```