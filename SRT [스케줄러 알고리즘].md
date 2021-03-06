## SRT [스케줄러 알고리즘]

#### 1. SRT가 무엇인가요?

SRT는 Shortest Remaining Time이라는 뜻으로 남아있는 시간이 짧은 프로세스를 먼저 실행한다는 뜻이다.
SRT는 선점형 스케줄러 알고리즘이기 때문에 중간에 인터럽트를 통해 다른 프로세스가 CPU를 선점할 수 있다.

#### 2. SRT의 동작방식

SRT는 남아있는 시간이 짧은 프로세스를 먼저 실행하기 때문에
준비 큐에 존재하는 모든 프로세스와 연산을 해야할 것만 같지만
이미 준비 큐에 들어와 있는 프로세스는 지금 실행되고 있는 프로세스보다 작업 시간이 길었기 때문에
있는 것이므로 비교를 하지 않는다.

그래서 새로 들어온 프로세스와 비교를 하게 되는데
새로 들어온 프로세스가 만약 현재 실행되고 있는 프로세스의 남은 실행 시간보다 짧다면
실행되고 있는 프로세스는 PCB의 상태를 저장하고 짧은 실행시간을 가진 프로세스가 CPU를 선점하게 된다.

다음은 프로세스이름, 실행시간, 도착시간을 표시한 준비 큐이다.

| 프로세스 이름 | 도착시간 | 실행시간 |
| :-----------: | :------: | :------: |
|       A       |    0     |    10    |
|       B       |    1     |    2     |
|       C       |    2     |    3     |
|       D       |    3     |    1     |
|       E       |    4     |    4     |

이에 대한 간트차트는 다음과 같다.
![OS_Study_Image7](./img/OS_Study_Image7.jpg)

이전의 간트차트와는 조금 다른 것을 알 수 있는데 SRT 스케줄러 알고리즘은 선점 방식으로 이루어지기 때문에
여러 프로세스의 점유율을 쉽게 보기 위해서 다음과 같이 설계하였다.
그리고 선점형이기 때문에 프로세스가 실행하다가 중지됐다가 다시 실행되는 경우가 있어
알고리즘이 살짝 복잡할 수 있다.

그럼 전체적인 동작을 한 번 살펴보겠다.
먼저, 0초에 도착한 프로세스는 프로세스 A 밖에 없으므로 프로세스 A가 실행되다가 실행시간이 1초 지났을 때,
프로세스 B가 도착하므로 프로세스 B의 실행시간인 2초와 실행하고 있던 프로세스 A의 실행시간 10-1초를
비교해보면 프로세스 B가 더 짧은 실행시간을 가지고 있으므로 프로세스 B를 먼저 실행한다.
프로세스 B의 실행시간이 1초가 지났을 때 프로세스 C가 도착하지만 프로세스 C의 실행시간은 3초이고
프로세스 B의 실행시간은 2-1초이ㅡ로 프로세스 B가 그대로 실행된다.

프로세스 B의 실행이 마무리 되고 3초에 프로세스 D가 들어오게 되는데
이때, 동작할 수 있는 프로세스는 A, C, D이다.
그런데 프로세스 A는 실행시간이 10-1초, 프로세스 C는 3초, 프로세스 D는 1초이다.
[지금 이 말을 하면서 위의 간트차트가 조금 잘못되었다는 것을 알게 되었다.
프로세스 C가 아니라 프로세스 D가 시작되어야 한다는 점이다.]
어쨌든 프로세스 D가 먼저 실행이 되고 종료가 되면 4초가 되어 프로세스 E가 도착하게 된다.
프로세스 A(10-1), C(3), E(4)이므로 프로세스 C가 실행되게 되고 이제 더 이상 도착하는 프로세스가 없으므로
프로세스 C가 끝까지 실행하고남은 프로세스 A와 프로세스 E를 살펴보면
프로세스 E가 더 적은 실행시간을 가지고 있으므로 먼저 실행하고 다음에 프로세스 A가 실행되게 된다.

#### 3. SRT의 특징

1. 선점형이기 때문에 CPU를 점유하고 있는 프로세스가 있어도 뺏길 수 있기 때문에
   어떠한 프로세스가 언제 종료될지를 예측할 수 없다.
2. 마찬가지로 선점형이기 때문에 프로세스의 CPU 점유자가 바뀔 때 마다
   프로세스의 PCB의 상태, CPU 레지스터의 정보를 저장하는 문맥 교환(Context Switching)이
   빈번하게 일어나기 때문에 오버헤드가 크다.
3. SJF와 마찬가지로 실행시간이 긴 프로세스는 영영 기아상태에 빠질 수 있다.
4. 비선점형, 선점형을 통틀어서 대기시간이 가장 짧은 스케줄러 알고리즘이다.