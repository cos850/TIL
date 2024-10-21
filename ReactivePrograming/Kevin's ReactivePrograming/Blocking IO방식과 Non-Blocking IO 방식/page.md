# Blocking I/O 방식과 Non-Blocking I/O 방식

## Blocking I/O 방식
![Blocking I/O](img/BlockingIO.png)

- 작업 스레드가 종료될 때까지 요청 스레드는 차단된다.
- 이러한 단점을 보완하기 위해 멀티 스레딩 기법을 사용할 수 있음 \
    => CPU 대비 많은 수의 스레드를 사용하는 애플리케이션은 비효율적이다.
    - [Context Switching](/Java/multi-thread/Process_Thread_Context%20Switch/page.md)으로 인한 스레드 전환 비용 발생
    - 메모리 사용에 있어 오버헤드 발생
    - 스레드 풀에서의 응답 지연 문제

## Non-Blocking I/O 방식
![Non-Blocking I/O](img/Non-BlockingIO.png)