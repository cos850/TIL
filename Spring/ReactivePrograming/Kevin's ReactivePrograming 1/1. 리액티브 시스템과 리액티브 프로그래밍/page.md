# 리액티브 프로그래밍이란?


## 특징
- 데이터 소스에 변경이 있을 때마다 데이터 전파
- 선언형 프로그래밍 패러다임 (실행할 동작을 구체적으로 명시하지 않고 목표만 정의함)
- 함수형 프로그래밍 기법 사용

## 명령형 프로그래밍 vs 선언형 프로그래밍
### 명령형 프로그래밍 예시
```java
List<Integer> numbers = Arrays.asList(1, 3, 21, 10, 8, 11);
int sum = 0;

for(int number : numbers){
    if(number > 6 && (number % 2 != 0)) {
        sum += number;
    }
}
```
### 선언형 프로그래밍 예시
```java
List<Integer> numbers = Arrays.asList(1, 3, 21, 10, 8, 11);

// 체인을 형성해서 작업을 선언
int sum = numbers.stream()
    .filter(number -> number > 6 && (number % 2 != 0))
    .mapToInt(number -> number)
    . sum();
```

## 리액티브 스트림즈란?
리액티브 프로그래밍을 표준화한 명세
[Github Reactive Stream 명세](https://github.com/reactive-streams/reactive-streams-jvm/blob/v1.0.3/README.md#specification)

- Publisher: 데이터 통제 주체
- Subscriber: Publisher가 통제한 데이터를 구독하는 구독자
- Subscription: 구독을 정의해둔 인터페이스
- Processor: Publisher와 Subscriber 역할을 동시에 할 수 있는 인터페이스

Reactive Stream을 구현한 구현체
- RxJava
- Java 9 Flow API
- Akka Streams
- Reactor
- 그 외 RxJS, RxScala, RxAndroid 등의 리액티브 확장

