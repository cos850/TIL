# 코루틴 예외처리

## launch 와 async 예외 처리 비교

### launch

예외를 발생되면 바로 예외를 출력하고 종료한다.

```kotlin
fun main(): Unit = runBlocking {
    
    val job = CoroutineScope(Dispatchers.Default).launch {
        throw IllegalArgumentException()
    }

    delay(1000)
}
```

```
Exception in thread "DefaultDispatcher-worker-1" java.lang.IllegalArgumentException
	at com.hyeri.learningcoroutine.lec01.ExceptionHandlingKt$main$1$job$1.invokeSuspend(ExceptionHandling.kt:7)
	at kotlin.coroutines.jvm.internal.BaseContinuationImpl.resumeWith(ContinuationImpl.kt:33)
	at kotlinx.coroutines.DispatchedTask.run(DispatchedTask.kt:104)
	at kotlinx.coroutines.scheduling.CoroutineScheduler.runSafely(CoroutineScheduler.kt:584)
	at kotlinx.coroutines.scheduling.CoroutineScheduler$Worker.executeTask(CoroutineScheduler.kt:811)
	at kotlinx.coroutines.scheduling.CoroutineScheduler$Worker.runWorker(CoroutineScheduler.kt:715)
	at kotlinx.coroutines.scheduling.CoroutineScheduler$Worker.run(CoroutineScheduler.kt:702)
	Suppressed: kotlinx.coroutines.internal.DiagnosticCoroutineContextException: [StandaloneCoroutine{Cancelling}@f8e3b0d, Dispatchers.Default]

Process finished with exit code 0
```


### async

launch 와 동일한 코드에서 async 로 변경하면 예외가 발생되지 않으면서 종료된다.

```kotlin
fun main(): Unit = runBlocking {
    val job = CoroutineScope(Dispatchers.Default).async {
        throw IllegalArgumentException()
    }

    delay(1000)
}
```

```

Process finished with exit code 0
```

이유는 async 의 특성에 있다.

async 는 예외를 바로 출력해서 종료하지 않기 때문이다. 
만약 예외를 확인하고 싶다면 다음과 같이 await 코드를 추가하면 된다.

```kotlin
fun main(): Unit = runBlocking {
    // 예외가 발생하지 않음
    val job = CoroutineScope(Dispatchers.Default).async {
        throw IllegalArgumentException()
    }

    delay(1000)
    job.await()
}
```

다음과 같이 예외가 발생한 것을 확인할 수 있다.
```
Exception in thread "main" java.lang.IllegalArgumentException
	at com.hyeri.learningcoroutine.lec01.ExceptionHandlingKt$main$1$job$1.invokeSuspend(ExceptionHandling.kt:13)
	at kotlin.coroutines.jvm.internal.BaseContinuationImpl.resumeWith(ContinuationImpl.kt:33)
	at kotlinx.coroutines.DispatchedTask.run(DispatchedTask.kt:104)
	at kotlinx.coroutines.scheduling.CoroutineScheduler.runSafely(CoroutineScheduler.kt:584)
	at kotlinx.coroutines.scheduling.CoroutineScheduler$Worker.executeTask(CoroutineScheduler.kt:811)
	at kotlinx.coroutines.scheduling.CoroutineScheduler$Worker.runWorker(CoroutineScheduler.kt:715)
	at kotlinx.coroutines.scheduling.CoroutineScheduler$Worker.run(CoroutineScheduler.kt:702)

Process finished with exit code 1
```

<br />

이번엔 async 를 runBlocking 에 자식 코루틴이 되도록 변경하고 실행해보자 !

```kotlin
fun main(): Unit = runBlocking {
    val job = async {
        throw IllegalArgumentException()
    }

    delay(1000)
}
```

이번엔 예외가 출력되면서 종료되었다!

```
Exception in thread "main" java.lang.IllegalArgumentException
	at com.hyeri.learningcoroutine.lec01.ExceptionHandlingKt$main$1$job$1.invokeSuspend(ExceptionHandling.kt:17)
	at kotlin.coroutines.jvm.internal.BaseContinuationImpl.resumeWith(ContinuationImpl.kt:33)
	at kotlinx.coroutines.DispatchedTask.run(DispatchedTask.kt:104)
	at kotlinx.coroutines.EventLoopImplBase.processNextEvent(EventLoop.common.kt:277)
	at kotlinx.coroutines.BlockingCoroutine.joinBlocking(Builders.kt:95)
	at kotlinx.coroutines.BuildersKt__BuildersKt.runBlocking(Builders.kt:69)
	at kotlinx.coroutines.BuildersKt.runBlocking(Unknown Source)
	at kotlinx.coroutines.BuildersKt__BuildersKt.runBlocking$default(Builders.kt:48)
	at kotlinx.coroutines.BuildersKt.runBlocking$default(Unknown Source)
	at com.hyeri.learningcoroutine.lec01.ExceptionHandlingKt.main(ExceptionHandling.kt:5)
	at com.hyeri.learningcoroutine.lec01.ExceptionHandlingKt.main(ExceptionHandling.kt)

Process finished with exit code 1
```

이유는 자식 코루틴의 예외는 부모 코루틴으로 전파되기 때문이다.

launch 도 동일하게 부모 코루틴에 예외를 전파하며, 
위 예제의 부모 코루틴인 runBlocking 은 예외를 출력하며 종료하는 성격을 가지고 있기 때문이다.

따라서, CoroutineScope를 통해 root 코루틴을 만들어 실행했을 때는 전파할 부모 코루틴이 없기 때문에 예외가 출력되지 않았던 것.

