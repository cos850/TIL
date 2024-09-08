# 멀티스레딩 - 스레드 스케줄링(Thread Scheduling), 동적 우선순위(Dynamic Priority)

프로세스, 스레드, 컨텍스트 스위치의 기본 개념은 아래 글 참고

[프로세스(Process), 스레드(Thread), 컨텍스트 스위치(Context Switch)](../Process_Thread_Context%20Switch/page.md)

<br />


## **1. 스레드 스케줄링(Thread Scheduling)**

각 스레드의 도착 시간과 작업 시간이 정해진 경우 스케줄링 비교


### 1-1. First Come First Server

> ✏️ 도착한 순서대로 실행

![스레드 스케줄링](./img/thread%20scheduling.png)

**[문제점]**

- 작업 시간이 긴 스레드가 먼저 도착 시 다른 스레드의 기아현상¹ 발생
- 특히 UI 스레드의 지연 시 어플리케이션의 응답성 방해하여 사용성에 최악임

<br />

> **💡 기아현상** <br />
> : 작업시간이 짧은 스레드가 작업시간이 긴 스레드에 의해 실행되지 못하는 상태
> 

<br />


### 1-2. Shortest Job First

> ✏️ 작업시간이 짧은 스레드를 먼저 실행

![thread scheduling - shortest job first](./img/thread%20scheduling-shortest%20job%20first.png)

**[문제점]**

- 작업 시간이 짧은 UI 이벤트는 자주 발생되어 계속 앞에 배치
- 연산이 필요한 긴 스레드는 영원히 실행하지 못함

> ⚠️ **위와 같은 문제점들로 인해 평등하게 스레드를 분배해서는 안된다.** 
> **이제 일반적으로 운영체제에서 사용하는 방법을 확인해보자.**

</aside>

### 1-3. 일반적인 운영체제 방법 - Epochs, Time Slice

> ✏️  **에포크**² 에 맞춰 시간을 적당한 크기로 나누고 스레드를 시간단위로 나누어 종류별로 에포크에 할당

![thread scheduling-epochs](./img/thread%20scheduling-epochs.png)

운영체제가 에포크에 맞게 시간을 적절히 나누고 스레드의 타임 슬라이스를 각 에포크에 할당한다.

모든 스레드가 에포크에 할당되어 실행되거나 완료되지 않는데, 운영체제가 각 스레드에 **동적 우선순위**를 적용해 시간을 할당한다.

> 💡 **에포크** (Epoch, = Unix Time) <br />
> : 1970년 1월 1일 00:00:00 UTC 부터 현재까지 누적된 초의 값, Unix를 개발한 벨 연구소에서 정의하여 Unix Time이라고도함 
> <br />
> ex) JAVA의 System.currentTimeMillis()
> 

<br />


### **2. 동적 우선순위(Dynamic Priority)**

> ✏️ 정적 우선순위 (Static Priority) + 보너스(Bonus)

- **정적 우선순위** : 개발자가 미리 설정해둔 우선순위
- **보너스** : 운영체제가 각각의 에포크마다 조절 (실시간 스레드, 인터렉티브 스레드, 이전 에포크에서 완료 못한 스레드에게 우선권 부여)

**=> 즉각적인 반응으로 응답성을 높이고, 기아 현상을 막는다.**