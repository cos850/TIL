# Reactor

## Reactor 용어 정리
- Publisher: 발행자, 생산자 (Emitter)
- Subscriber: 구독자, 소비자
- Emit: Publisher가 데이터를 내보내는 것 (방출하다, 내보내다)
- Sequence: Publisher가 Emit하는 데이터의 연속적인 흐름을 정의해 둔 것
- Subscribe: Subscriber가 Sequence를 구독하는 것
- Dispose: Subscriber가 Sequence 구독을 해지하는 것

<br />

## 코드로 보는 Reactor
```java
Mono.just("Hello Reactor!") // publisher 가 데이터를 제공
    .subscribe(message -> System.out.println(message)); // subscriber 가 데이터 받음

Flux.just("Hello", "Reactor")
    .map(message -> message.toLowerCase())
    .subscribe(message -> System.out.print(message + " "));
```