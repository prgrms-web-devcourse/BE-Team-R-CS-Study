# 1. 스케줄러

스케줄러란 어떤 프로세스에게 자원을 할당할지를 결정하는 운영체제 커널의 코드를 지칭한다.
<br><br>

## (1) 프로세스 상태 종류

### ① new : 프로세스 생성중

- 프로세스를 위한 자료구조는 생성 완료
- 메모리 획득은 승인받지 못함

### ② ready : 준비가 다 된 프로세스가 CPU를 기다리는 상태

- 프로세스가 메모리 적재된 상태로 실행하는데 필요한 자원을 모두 얻은 상태
- 아직 CPU는 받지 못함
- ready 상태인 프로세스는 여러개가 존재할 수 있음

### ③ running : 프로세스가 CPU를 할당받아, 명령어을 실행하고 있는 상태

- CPU가 하나이면 실행중인 프로세스는 매 시점 하나뿐임

### ④ blocked : CPU를 할당받아도 당장은 명령을 실행할 수 없는 상태

- ex) 입출력 작업이 진행 중인 경우

### ⑤ terminated : 프로세스 실행 종료

- 프로세스는 종료되었으나, 운영체제가 프로세스와 관련된 자료구조를 완전히 정리하지 못함

### ⑥ suspended : 프로세스 중지 상태

- suspended 상태의 프로세스는 메모리를 강제로 뺏긴 상태로 특정한 이유로 프로세스의 수행이 정지된 상태를 의미함
- 외부에서 다시 재개시키지 않는 이상 다시 활성화 될 수 없음
- 중기 스케줄러에 의해 디스크로 스왑 아웃된 프로세스의 상태가 대표적인 suspended상태라 할 수 있음
- Suspended Ready : ready 상태의 프로세스가 디스크로 swap out
- Suspended  Blocked : blocked 상태의 프로세스가 디스크로 swap out

<br><br>
## (2) 프로세스를 스케줄링하기 위한 큐

### ① Ready Queue (준비 큐)

- 준비 상태에 있는 프로세스들을 줄 세우기 위한 큐

### ② Device Queue (장치 큐)

- 운영체제는 특정 자원을 기다리는 프로세스들을 줄 세우기 위해 자원 별로 device queue를 둔다.
- ex) disk I/O queue

### ③ Job Queue (작업 큐)

- 시스템 내의 모든 프로세스를 관리하기 위한 큐
- 프로세스의 상태와 무관하게 현재 시스템 내에 있는 모든 프로세스가 작업 큐에 속하게 된다.

<br><br>
## (3) 스케줄러 종류

![Untitled](https://user-images.githubusercontent.com/50768959/163463210-ee5f1bf0-cde5-49b9-bc0b-99160b3af31c.png)

출처 : [https://technote-mezza.tistory.com/70](https://technote-mezza.tistory.com/70)

### ① 장기 스케줄러 (Long-term scheduler / Job scheduler)

한정된 메모리에 많은 프로세스들이 단기간에 메모리에 올라올 경우, 디스크에 임시로 저장된다. 디스크에 저장되어있는 프로세스 중 어떤 프로세스에 메모리를 할당하여 ready queue로 보낼지 결졍하는 역할을 한다.

- 메모리와 디스크 사이의 스케줄링을 담당
- 프로세스에 메모리 및 리소스를 할당한다.
- 실행중인 프로세스의 수를 제어한다.
- 프로세스의 상태 : **new → ready**
- 수십 초 내지 수 분 단위로 가끔 호출됨 → 상대적으로 속도가 느려도 된다.

### ② 단기 스케줄러 (Short-term scheduler / CPU scheduler)

준비 상태의 프로세스 중에서 어떤 프로세스를 다음번에 실행 상태로 만들 것인지 결정한다.

→ Ready queue의 프로세스들 중 어떤 프로세스에게 CPU를 할당할 것인가를 결정한다.

- 프로세스에 CPU를 할당
- 프로세스의 상태 : **ready → running → waiting → ready** (이라고 나와있는데 ready → running만 아닌가..?)
- 밀리초 정도의 시간 단위로 매우 빈번하게 호출됨 → 수행 속도가 충분히 빨라야 함

### ③ 중기 스케줄러 (Medium-term scheduler / Swapper)

- 실행중인 프로세스의 수를 제어한다.
- 여유 공간 마련을 위해 프로세스를 통째로 메모리에서 디스크로 쫓아냄 (swapping)
- blocked 상태는 다른 I/O 작업을 기다리는 상태이기 때문에 스스로 ready state로 돌아갈 수 있지만 suspended상태는 외부적인 이유로 suspending 되었기 때문에 스스로 돌아갈 수 없다.
    
    (I/O 작업때문에 blocked된 애는 I/O작업을 끝내면 “나 다했어!”하고 스스로 ready state로 돌아가지만, swapped out된 애들은 사용자가 그걸 필요로 할 때까지 무한 대기해야함!)
    
- 프로세스의 상태 : **ready ↔ suspended, blocked ↔ suspended**

<br><br>
# 2. CPU 스케줄러 종류

⭐ 스케줄링 효율성 비교 기준

- Turnaround time : 종료된 시간 - 처음 생성된 시간 (ready queue에 들어온 시간)
- Response time : 처음 프로세스가 실행된 시간 - 처음 생성된 시간 (ready queue에 들어온 시간)

<br>

### ① First Come, First Served (FCFS)

- 이름 그대로 먼저 들어온 애들을 먼저 실행한다.
- **비선점형**(Non-Preemptive) 스케줄링 : 프로세스들이 CPU를 한번 잡으면 본인 일을 다 끝낼 때 까지 CPU를 반환하지 않음

<img src = "https://user-images.githubusercontent.com/50768959/163463325-2bb59b66-fcc9-4abb-a31c-519d0fdba6e6.png" width="600">


- 이 경우 평균 turnaround time은 20이 된다.
- 하지만 다음 같은 경우에는 평균 turnaround time은 110이 된다.

<img src = "https://user-images.githubusercontent.com/50768959/163463329-2918445b-4f30-4c0a-97ad-b0e21e0a750e.png" width="600">

→ Convoy Effect : 소요 시간이 긴 프로세스가 먼저 도달하여 효율성을 낮추는 현상

<br>

### ② Shortes Job First (SJF)

(얘는 대부분 정리되어 있는 것과 제가 학교 강의로 들은 내용이랑 달라서 논의가 필요합니다!)

- 이름 그대로 실행시간이 짧은 것 부터 실행한다.
- **비선점형** 스케줄링

<img src = "https://user-images.githubusercontent.com/50768959/163463380-0be3d378-317b-4c4f-b372-ff7aac823535.png" width="600">


- 이 경우 평균 turnaround time이 50으로 감소하게 된다.

→ 하지만 이 경우는 모든 프로세스가 동시에 들어왔을 때만 성립한다.

<img src = "https://user-images.githubusercontent.com/50768959/163463383-d2d4d6c4-de22-4695-ba8c-c97998799344.png" width="600">

- 이 경우, A가 먼저 들어와서 **Convoy effec**t 가 여전히 발생한다.
- 또한, 사용시간이 긴 프로세스는 거의 영원히 CPU를 할당받을 수 없는 **Starvation** 문제가 발생할 수 있다.

<br>

### ③ ****SRTF(Shortest Remaining Time First) == STCF (Shortest Time to Completion First)****

(SJF의 선점형 버전)

- 새로운 프로세스가 도착할 때마다 새로운 스케줄링이 이루어진다.

→ 새로운 프로세스까지 포함해서 가장 남은 시간이 짧은 프로세스부터 실행한다.

- **선점형**(Preemptive) 스케줄링 : 현재 수행중인 프로세스한테서 CPU를 뺏을 수 있다.

<img src = "https://user-images.githubusercontent.com/50768959/163463433-c31c3da6-8cfa-4635-ba78-54eeaa9db7e5.png" width="600">


- Starvation 현상 : 사용시간이 긴 프로세스는 오랫동안 CPU를 할당받지 못할 수 있다.
- 남은 CPU burst time을 예측하기가 힘들다.

<br>

### ④ Priority Scheduling

- 우선순위가 가장 높은 프로세스에게 CPU를 할당하는 스케줄링
- (우선순위 알고리즘은 다양하다.)
- 선점형 스케줄링(Preemptive) 방식 : 더 높은 우선순위의 프로세스가 도착하면 실행중인 프로세스를 멈추고 CPU 를 선점한다.
- 비선점형 스케줄링(Non-Preemptive) 방식 : 더 높은 우선순위의 프로세스가 도착하면 Ready Queue 의 Head 에 넣는다.
- Starvation : 우선순위가 낮은 프로세스는 계속해서 CPU를 얻지 못한다.
    
    → 해결방법 : aging → 아무리 우선순위가 낮은 프로세스라도 오래 기다리면 우선순위를 높여주자. (방법은 여러가지가 있다)
    
<br>

### ⑤ Round Robin

- 현대적인 CPU 스케줄링
- 각 프로세스는 동일한 크기의 할당 시간(time quantum)을 갖게 된다.
- 할당 시간이 지나면 프로세스는 선점당하고 ready queue 의 제일 뒤에 가서 다시 줄을 선다.
- CPU 사용시간이 랜덤한 프로세스들이 섞여있을 경우에 효율적
- 프로세스의 context 를 save 할 수 있기 때문에 가능하다. → Context Switching이 이루어질 수 있다.
- 장점
    - Response time이 빨라진다 : n 개의 프로세스가 ready queue 에 있고 할당시간이 q(time quantum)인 경우 각 프로세스는 q 단위로 CPU 시간의 1/n 을 얻는다. 즉, 어떤 프로세스도 (n-1)q time unit 이상 기다리지 않는다.
    - 프로세스가 기다리는 시간이 CPU 를 사용할 만큼 증가한다 : 공정한 스케줄링이라고 할 수 있다.
- 주의할 점
    - 설정한 time quantum이 너무 커지면 FCFS와 같아진다.
    - 너무 작아지면 스케줄링 알고리즘의 목적에는 이상적이지만 잦은 context switch 로 overhead 가 발생한다.
    
    → 적당한 time quantum을 설정하는 것이 중요함
