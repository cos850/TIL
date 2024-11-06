# BackPressure

## Publisher 와 Subscriber 간의 프로세스

![flow](flow.png)

1. subscribe() 를 통해 구독 시작
2. publisher가 onSubscribe 시그널을 통해 정상적으로 구독이 이루어졌음을 보냄
3. subscriber가 publisher에게 request 시그널을 보냄
4. publisher는 onNext라는 시그널을 전달하면서 데이터 emit \
4-1. Subscriber가 emit 된 데이터의 처리가 끝나면 다시 request 시그널을 보냄 \
4-2. onNext를 통해 데이터 emit. 4-1 ~ 4-2 과정 반복
5. 더이상 보낼 데이터가 없으면 onComplete 시그널을 Subscriber에게 전달한다


## Backpressure 란?
Publisher에서 emit되는 데이터를 Subscriber 쪽에서 안정적으로 처리하기 위한 제어기능

 ![backpressure](backpressure.png)

publisher에서 emit되는 속도가 빠르나, Subscriber에서 처리하는 속도는 느린 경우
데이터가 유실되거나, 데이터가 많이 쌓여 시스템에 과부화가 올 수 있다.


## Reactor에서 Backpressure 처리

1. 요청데이터의 개수 제어
    
    : Subscriber가 적절히 처리할 수 있는 수준의 데이터 개수를 Publisher에게 요청
    - Subscriber -데이터_2개_요청-> Publisher
    - Publisher -데이터2개emit-> Subscriber
    - Subscriber 데이터 처리
    - Subscriber -데이터_2개_요청-> Publisher
    - ... 반복
