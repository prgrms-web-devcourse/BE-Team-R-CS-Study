## DNS(Domain Name System)

- IP 주소 테이블을 가지고 있는 서버
- 사용자가 웹 사이트에 접속하기 `google.com` 과 같은 도메인을 입력하지만, 원칙적으로는 IP 주소를 통하여 접속을 해야함. DNS는 여기서 도메인이 가리키는 IP 주소를 브라우저에게 반환하여 접속을 가능하게 함.

![image](https://user-images.githubusercontent.com/72663337/175243348-e3edf541-4f9a-4791-8c6b-59082af986a5.png)

`DNS 서버(https://www.geeksforgeeks.org/working-of-domain-name-system-dns-server/)`




### Round Robin
- 프로세스에 CPU를 할당하는 스케줄링 기법 중 하나.
- 시분할 시스템, 일정한 시간단위로 CPU 할당
- 프로세스 간의 우선순위를 두지 않음.


### DNS의 round robin
- DNS 서버 구성되는 방식 중 하나.
- 도메인에 대한 IP 요청 시 round-robin 방식을 통하여 IP가 반환
  - 요청 시마다 DNS에서 반환되는 IP가 바뀜
- DNS를 통하여 서버 트래픽을 분산시킴.
  - 로드 밸런서와 같은 부하를 분산해 주는 중간 장비가 필요하지 않다. (자동적으로 반환되는 IP 값이 바뀌며 부하가 분산되기 때문)



![image](https://user-images.githubusercontent.com/72663337/175243527-14d77474-756e-4057-9752-1510ee60fe2f.png)


`DNS Round Robin (https://byeongyeon.tistory.com/80)`




### 단점
- 균등한 트래픽의 분산이 어려움
  - 도메인에 대한 IP 주소를 요청하는 브라우저 등에서 DNS로부터 전달받은 주소 결과를 캐싱 -> 다음 요청 시에 DNS에서 반환되는 IP 주소 값이 변하고, 따라서 웹 브라우저에서 사용하는 IP 주소도 바뀌어야 하지만 캐싱되어 있는 값 때문에 그렇지 못함.
  - 한 클라이언트에 대한 요청은 분산될 수 있지만, 여러 사람이 요청을 보내는 경우에는 균등한 분산이 이루어진다고 보장할 수 없음 
- 서버가 다운되어도 확인이 불가능함
  - 로드밸런서의 경우에는 서버들의 상태를 관찰하며 서버가 제대로 작동하는지 여부를 판단하며 분산을 조절할 수 있는데, DNS에서는 이런 작업을 할 수 없음






### 극복 방법

1. Round robin 방식을 보완한 방식


#### 다중화 구성 방식(Synchronous Time-Division Multiplexing)

ex : AWS Route53, Dyn DNS 서비스

AP 서버에 VIP(Virtual IP)를 부여하여 다중화를 구성하고, 

각 AP 서버를 **Health check** 후 이상이 감지되면 VIP를 정상 AP 서버로 인계하는 방식을 사용함.



-> DNS 서버에 실시간으로 서버의 상태를 확인할 수 있는 값을 추가하여, 이 값을 통하여 서버 상태를 확인(Health check)하고 우회 루트를 제공하거나, 에러를 전송하는 방식







2. 다른 DNS 스케줄링 알고리즘
#### 가중치 편성 방식 알고리즘(Weighted round robin)

각 웹 서버에 가중치를 부여하여 분산 비율을 정함.

처리 능력이 높을수록 가중치가 크게 설정됨.



#### Least Connnection

접속 클라이언트 수가 가장 적은 서버의 IP 주소를 반환함.

로드밸런서가 접속 정보를 알려주는 작업을 수행해야 함.


<br>



### 결론

여러 웹 서버로 트래픽을 분산시키는 단순하고 편리한 방식.

가용성이 필요한(항상 서비스되어야 하는) 서비스에 round robin 방식을 사용하는 것은 좋지 않다.

가용성이 요구되는 경우, 다중화 구성 방식이 적용되는 게 권장된다.






#### Reference

https://velog.io/@dbstjrwnekd/DNS-Round-Robin





http://dailusia.blog.fc2.com/blog-entry-362.html

