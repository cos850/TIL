# Cold Sequence vs Hot Sequence


## Cold Sequence
### 데이터 흐름 예시
1. Cold Publisher는 첫 번재 구독이 발생했을 때 1 , 3, 5, 7 이라는 데이터를 emit 한다.
2. 첫 번째 구독의 1을 emit 했을 때 쯤 2번째 구독이 발행한다.
3. 두 번째 구독한 Subscriber에게도 똑같이 1, 3, 5, 7 데이터를 emit 한다.


<br />

> [!NOTE]
> Subscriber가 구독할 때마다 타임라인의 처음부터 모든 emit 데이터를 받을 수 있다 (타임라인이 계속 생기는 것과 같음)


<br />

### 예시 코드
```java
Flux<String> coldFlux = Flux.fromIterable(Arrays.asList("A", "B", "C"))
    .delayElements(Duration.ofSeconds(1))
    .map(String::toLowerCase);

coldFlux.subscribe(emit -> System.out.println("# Subscriber1 = " + emit));
Thread.sleep(1500);

coldFlux.subscribe(emit -> System.out.println("# Subscriber2 = " + emit));
Thread.sleep(3500);

//# Subscriber1 = a
//# Subscriber1 = b
//# Subscriber2 = a // 2번째 Subscriber가 a부터 c까지 모든 요소를 받아옴
//# Subscriber1 = c
//# Subscriber2 = b
//# Subscriber2 = c
```

<br />
<br />

## Hot Sequence
### 데이터 흐름
1. Hot Publisher는 첫 번재 구독이 발생했을 때 1 , 3, 5, 7 이라는 데이터를 emit 한다.
2. 첫 번째 구독의 1을 emit 했을 시점에 2 번째 구독이 발생한다
3. 그러면 두번째 Subscriber는 1은 받지 못하고 3 부터 emit 받을 수 있다.

<br />

> [!NOTE]
> 첫 번째 Subscriber는 모든 데이터를 받을 수 있지만, 두 번째 Subscriber는 참여한 타임라인의 시점의 데이터부터 받을 수 있다. (구독 횟수에 상관 없이 타임라인이 하나만 생긴다)

<br />

### 예시 코드
```java
Flux<String> hotFlux = Flux.fromIterable(Arrays.asList("A", "B", "C"))
    .delayElements(Duration.ofSeconds(1))
    .share();   // shere 가 중요 !! Cold Sequence > Hot Sequence 로 변경 (여러 Subscriber가 해당 Flux를 공유함)

hotFlux.subscribe(emit -> System.out.println("# Subscriber1 = " + emit));
Thread.sleep(1500);

hotFlux.subscribe(emit -> System.out.println("# Subscriber2 = " + emit));
Thread.sleep(3500);

//# Subscriber1 = A
//# Subscriber1 = B
//# Subscriber2 = B // 2번째 Subscriber가 B부터 요소를 받아옴
//# Subscriber1 = C
//# Subscriber2 = C
```
