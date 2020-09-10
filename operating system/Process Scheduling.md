# Process Scheduling

# CPU 스케쥴링

---

Category : C.S.

![Process%20Scheduling%2091da0e9eefd6446c9493ce53b6465ed7/Untitled.png](Process%20Scheduling%/Untitled.png)

출처: 유준, Operating System, 2018

### 정의

- 여러 프로세스가 번갈아가며 사용하는 자원을 어떤 시점에 어떤 프로세스에게 자원을 할당할 지 결정하는 것
- 메모리에 올릴 실행 준비가 된 프로세스(스레드)를 선택하고, 그 중 하나를 CPU에 할당하는 **행위**
    - 여기서 행위란...
        - 어떤 프로세스를 다음에 실행해야 하는가
        - 그 프로세스를 얼만큼 실행해야 하는가

### CPU 스케쥴러

- CPU 스케쥴링을 동작하는 **OS 구성요소**

### Dispatcher

- CPU의 자원을 CPU 스케쥴러에 의해 선택된 프로세스에게 할당해주는 **CPU Scheduler module**
    - Context Switching
    - User mode로 전환
    - 프로세스의 PC 값에 해당하는 코드를 실행해 프로세스 재시작

[스케줄링(Scheduling) 개념 및 이해](https://www.crocus.co.kr/1373?category=268776)

- **1단계 :: 작업 스케줄링**

    이 단계는 실제로 시스템 자원을 사용할 작업을 선택하는 단계이다.

    디스크에 프로그램으로 있는 작업 중 프로세스화할 작업과 시스템으로 들어갈 작업을 결정하므로 승인 스케줄링이라고 한다.

    이는 작업 스케줄링에 따라 작업을 프로세스들로 나누어 생성한다.

    이는 수행 빈도가 적기 때문에 **장기 스케줄링**이라고도 부른다.

    디스크에서 **메모리로 작업(프로그램)을 가져와 PCB를 부착시켜 메모리를 적재하여 프로세스로 만든 후 Ready Queue**에 넣는다.

- **2단계 :: 작업 승인과 프로세서 결정 스케줄링**

    이 단계는 프로세서(CPU)를 사용할 프로세스를 결정하는 작업 승인을 해주는 역할을 한다.

    혹은 시스템의 오버헤드가 심하면 연기(보류)할 프로세스를 결정한다.

    따라서 이 2단계는 1, 3단계의 완충 역할을 한다.

    이는 수행빈도가 1단계, 3단계의 사이라 **중기 스케줄링**이라고도 부른다.

- **3단계 :: 프로세서 할당 스케줄링**

    3단계는 dispatcher가 Ready Queue에 있는 프로세스 중에서 우선순위에 따라 프로세스를 프로세서에 할당해주는 스케줄링이다.

    이는 다음 프로세스 결정을 자주 해야하므로 **단기 스케줄링**이라고도 부른다.

    장기 스케줄러에서 메모리에 적재시킨 프로세스 중 프로세서(CPU)를 할당하여 실행 상태가 되도록 결정하는 스케줄링을 한다. 이때 프로세스가 요청하는 자원의 할당을 만족해야 한다.

### Process State Transition

![Process%20Scheduling%2091da0e9eefd6446c9493ce53b6465ed7/Untitled%201.png](Process%20Scheduling%2091da0e9eefd6446c9493ce53b6465ed7/Untitled%201.png)

출처: 유준, Operating System, 2018

- 3가지 상태, ready, running, waiting
- CPU 스케쥴러는 아래의 경우 반드시 작동해야한다.
    1. 프로세스가 Running → Waiting 으로 상태 변화를 할 때,
    2. 프로세스가 Terminated 되었을 때

### Preemptive(선점) vs Non-preemptive(비선점)

- 선점
    - 현재 running state의 프로세스가 실행 중 인터럽트가 발생할 수 있고,
    - OS에 의해 ready state로 이동하는 경우
    - 대부분의 현대 운영체제가 이에 해당됨
        - Windows 95, Windows 10, Mac OS X Linux
    - 선점 스케쥴링의 경우 적은 대기 시간(response time)으로 인해 실시간 시스템에 용이하지만 높은 빈도의 context switch 발생으로 인한 overhead 증가, 자원의 경쟁 상태에 대한 처리 등 복잡한 기법을 요구한다.
    - 참고로 선점 스케줄링은
        - 1. 인터럽트 핸들링 후
        인터럽트 핸들러를 실행한 후 실행을 멈춘 프로세스로 복귀 하기 전
        - 2. 시스템 콜 핸들링 후
        시스템 콜 함수를 처리한 후 유저 공간으로 복귀하기 직전에 발생한다

- 비선점
    - CPU에 프로세스가 할당 되면
        - 스스로 자원을 반환하는 경우(즉 Terminated된 상황)
        - 스스로 I/O 작업을 해 self-blocked 되어 waiting 상태가 된 경우
    - 이외에는 할당된 CPU자원을 반환하지 않는다.
        - 옛날 운영체제가 이에 해당함
            - Windows 3.0
    - 비선점 스케쥴링의 경우 적은 context switch 발생으로 인해 overhead가 적지만 프로세스의 대기 시간(response time)이 길어진다

### 스케쥴링 기준(Criteria)

[CPU Scheduling, Process 이해하기](https://zzsza.github.io/development/2018/07/29/cpu-scheduling-and-process/)

- CPU Utilization (CPU 이용률) : CPU가 얼마나 놀지않고 부지런히 일하는가
- Throughput (처리율) : 시간당 몇 개의 작업을 처리하는가
- Turnaround time (반환시간) : 작업이 레디큐에 들어가서 나오는 시간의 차이(병원에서 진료 받을 때..대기하고 CT 찍고, … 나오는 시간 차) 짧아야 좋음
- Waiting time (대기시간) : CPU가 서비스를 받기 위해 Ready Queue에서 얼마나 기다렸는가
- Response time (응답시간) : Interactive system에서 중요. 클릭-답, 타이핑-답. 첫 응답이 나올 때 까지 걸리는 시간

### 스케쥴링 알고리즘

- Ready queue의 프로세스(스레드) 중 어떤 프로세스를 CPU에 할당할 것인지 정하는 알고리즘
    - First-Come, First-Served (FCFS) Scheduling
    - Shortest-Job-First (SJF) Scheduling
    - Round-Robin Scheduling
    - Priority Scheduling
    - Earliest Deadline First Scheduling

### FCFS Scheduling

![Process%20Scheduling%2091da0e9eefd6446c9493ce53b6465ed7/Untitled%202.png](Process%20Scheduling%2091da0e9eefd6446c9493ce53b6465ed7/Untitled%202.png)

출처: 유준, Operating System, 2018

- **FIFO 큐**를 사용한 **비선점** 스케쥴링

![Process%20Scheduling%2091da0e9eefd6446c9493ce53b6465ed7/Untitled%203.png](Process%20Scheduling%2091da0e9eefd6446c9493ce53b6465ed7/Untitled%203.png)

출처: 유준, Operating System, 2018

- 평균 대기 시간(Average Waiting time) = (0 + 24 + 27) / 3 = 17
- 평균 반환 시간(Average Turnaround time) = (24 + 27 + 30) / 3 = 27

![Process%20Scheduling%2091da0e9eefd6446c9493ce53b6465ed7/Untitled%204.png](Process%20Scheduling%2091da0e9eefd6446c9493ce53b6465ed7/Untitled%204.png)

출처: 유준, Operating System, 2018

- 평균 대기 시간(Average Waiting time) = (6 +0 + 3) / 3 = 3
- 평균 반환 시간(Average Turnaround time) = (30 + 3 + 6) / 3 = 13

    ➡️ **스케쥴링 알고리즘에 의해 W.T. 와 T.T.를 줄일 수 있다.**

- 장점
    - Context Switch가 최소한으로 일어난다.
    - Ready Queue에 있는 모든 프로세스가 실행될 수 있으므로 Starvation이 없다.
    - 구현하기 쉽다.
- 단점
    - 비선점식이라 대화식, 실시간 프로세스에는 부적합하다.
    - Convoy Effect가 발생할 수 있다.
    - 입력된 프로세스의 CPU 버스트 타임에 따라 W.T. 와 T.T.가 영향을 많이 받는다.

### Shortest-Job-First (SJF) Scheduling

- CPU 버스트 시간(CPU 자원을 사용하는 시간)이 **짧은 순서**를 기준으로 스케쥴링
- 비선점 Scheme
    - 한번 CPU에 할당 되면 해당 프로세스가 요구하는 CPU자원을 전부 사용함
- 선점 Scheme
    - CPU 버스트 시간이 현재 Running 중인 CPU의 잔여 CPU 버스트 시간보다 작은 새로운 프로세스가 ready queue에 오면 그 프로세스가 CPU 자원을 선점함 ➡️ **SRTF**

![Process%20Scheduling%2091da0e9eefd6446c9493ce53b6465ed7/Untitled%205.png](Process%20Scheduling%2091da0e9eefd6446c9493ce53b6465ed7/Untitled%205.png)

출처: 유준, Operating System, 2018

## Q1. SRTF 스케쥴링 시 A.W.T 와 A.T.T. 를 구하시오

![Process%20Scheduling%2091da0e9eefd6446c9493ce53b6465ed7/Untitled%206.png](Process%20Scheduling%2091da0e9eefd6446c9493ce53b6465ed7/Untitled%206.png)

![Process%20Scheduling%2091da0e9eefd6446c9493ce53b6465ed7/Untitled%207.png](Process%20Scheduling%2091da0e9eefd6446c9493ce53b6465ed7/Untitled%207.png)

출처: 유준, Operating System, 2018

- 장점
    - 평균 대기 시간이 최소가 된다(항상 실행 시간이 짧은 작업을 가장 먼저 실행하기 때문)
    - Ready Queue의 대기 프로세스의 수를 최소화 할 수 있다.
- 단점
    - 불공정, 기아(Starvation) 현상이 발생할 수 있다.
        - CPU 버스트 시간이 긴 프로세스에게 불공정하다.
        - 짧은 CPU 버스트 시간을 가진 프로세스가 CPU를 계속 잡을 수 있기 때문.
        - 실행시간을 예측하기 어렵다 ➡️ 사실 CPU 버스트 시간을 CPU가 정확히 알 수 있는 방법은 없다.

## Round Robin (RR)

![Process%20Scheduling%2091da0e9eefd6446c9493ce53b6465ed7/Untitled%208.png](Process%20Scheduling%2091da0e9eefd6446c9493ce53b6465ed7/Untitled%208.png)

출처: 유준, Operating System, 2018

- SJF의 단점인 기아 현상을 해결한 스케쥴링 알고리즘
- 쪼갠 동일한 시간을 Time Quantum, Time Slice라 부름
    - Time quantum 시간양자 = time slice (10 ~ 100msec)
- Time Quantum(time slice)이후에 다음 프로세스가 CPU를 선점하게 한다.
    - Time Quantum이 지나면 interrupt를 발생시켜 Context switch를 만든다.
- 선점 뒤에는 FIFO 큐에 들어온다.

- 장점
    - 모든 프로세스가 공정하게 실행되어 기아 현상이 발생하지 않는다.
    - Process의 CPU 버스트 시간이 다양할 때 평균적으로 W.T.을 작게 가져갈 수 있다.

- 단점
    - Time Slice의 사이즈에 민감하다.
        - 동일한 CPU 버스트 시간을 가진 프로세스 2개가 작업을 한다면....
        - 엄청난 Context Switch로 이어지고, 오버헤드가 발생한다.
        - 만일 Context Switch에 소모되는 자원이 없다하더라도 평균 T.T. 는 엄청나게 커진다
    - 하드웨어 타이머가 필요하다.

    ### Priority Scheduling

    - 우선순위 기반의 스케쥴링
    - 그러므로 SJF 도 우선순위 스케쥴링의 일종이다.
    - 선점, 비선점 둘 다 가능
    - 선점인 경우는 우선 순위가 높은 프로세스를 CPU에 우선적으로 할당함
    - 비선점인 경우는 우선 순위가 높은 프로세스를 큐의 맨 앞에 넣어줌
    - 문제점
        - 기아 현상
    - 해결책
        - Aging
            - 우선 순위가 높은 프로세스가 계속 들어오면 우선 순위가 낮은 프로세스는 실행이 불가능 함.
            - 오래 대기하는 프로세스가 우선 순위를 점진적으로 증가시켜주는 방법을 이용함

    ### Multilevel Queue

    ![Process%20Scheduling%2091da0e9eefd6446c9493ce53b6465ed7/Untitled%209.png](Process%20Scheduling%2091da0e9eefd6446c9493ce53b6465ed7/Untitled%209.png)

    - Ready queue를 여러 개의 큐로 분리해서 관리한다.
    - 커널은 우선 순위가 높고, 비어있지 않은 큐의 프로세스를 우선적으로 실행한다.
    - 즉 우선 순위가 높은 큐가 빌 때 까지 낮은 우선순위 큐에 있는 프로세스를 실행할 수 없다.
    - 큐를 여러개 사용하고, 스케쥴링 알고리즘이 각자 적용되기 때문에 추가 오버헤드가 발생할 수 있다. 그러나 응답이 빠르다.

    ### Multiple-Processor Scheduling

    ![Process%20Scheduling%2091da0e9eefd6446c9493ce53b6465ed7/Untitled%2010.png](Process%20Scheduling%2091da0e9eefd6446c9493ce53b6465ed7/Untitled%2010.png)

    출처: 유준, Operating System, 2018

    - CPU는 일반적으로 여러 개 사용한다.
    - 프로세스가 실행될 때 매번 다른 CPU에서 실행되는 것은 비효율 적이다.
        - Cache Miss의 원인이 됨
    - Affinity Scheduling
        - 프로세스의 running 을 동일한 CPU에서 하도록 유지하는 스케쥴링
        - 주의할 점
            - 이 경우 작업을 하지 않는 CPU가 생길 가능성이 있다.
    - Load Balancing
        - Idle한 CPU에 busy한 CPU의 프로세스를 할당해주는 작업

### Real-time scheduling

- **실시간 프로그램**에서 고려해야할 사항이 **deadline**이다.
- **주기적**으로 이벤트가 발생하고, 그 이벤트를 **일정 시간 내에 처리**해야한다.
- 그 일정 시간이 deadline인데, 프로세스를 이 deadline 이전에 실행 완료 시켜주는 것이 필요하다.

![Process%20Scheduling%2091da0e9eefd6446c9493ce53b6465ed7/Untitled%2011.png](Process%20Scheduling%2091da0e9eefd6446c9493ce53b6465ed7/Untitled%2011.png)

출처: 유준, Operating System, 2018

- **Rate Montonic Scheduling**
    - 프로세스의 실행 주기가 짧은 프로세스에 우선적으로 CPU 할당
    - 즉 실행 주기의 역수를 기준으로 삼음
    - 문제점
        - 주기가 상대적으로 긴 프로세스가 CPU를 할당 받지 못해 작업을 마치지 못할 수 있다.
        - 해결법 - deadline을 기준으로 CPU를 할당하자
            - ➡️ **Earliest Deadline First Scheduling(EDF)**

![Process%20Scheduling%2091da0e9eefd6446c9493ce53b6465ed7/Untitled%2012.png](Process%20Scheduling%2091da0e9eefd6446c9493ce53b6465ed7/Untitled%2012.png)

출처: 유준, Operating System, 2018
