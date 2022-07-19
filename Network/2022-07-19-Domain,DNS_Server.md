## Domain 이란?

<aside>
💡 12자리의 IP 주소를 문자로 표현한 주소

</aside>

- IP 주소는 12자리의 숫자로 되어 있기 때문에 사람이 외우기 힘들다는 단점이 있다.
- 이러한 문제를 해결하기 위해 IP 주소를 사람이 기억하기 쉬운 문자로 표현한 주소

![https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fb0r55g%2FbtqN014kHQs%2FTTCKKIQVaY5aj5Pp8mIZQK%2Fimg.png](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fb0r55g%2FbtqN014kHQs%2FTTCKKIQVaY5aj5Pp8mIZQK%2Fimg.png)

- 도메인은 ‘naver.com’처럼 몇 개의 의미있는 문자들과 .의 조합으로 구성된다.

> IP 주소
- IP 주소란 많은 컴퓨터들이 인터넷 상에서 서로를 인식하기 위해 지정받은 식별용 번호
>

## DNS (Domain Name System)

<aside>
💡 인터넷 주소창에 Host Domain Name을 입력했을 대 해당 문자를 IP 주소로 변환해 주는 시스템

</aside>

- 도메인은 사람의 편의성을 위해 만든 주소이므로 실제로는 컴퓨터가 이해할 수 있는 IP 주소로 변환하는 작업이 필요하다.
- 이때, 사용할 수 있도록 미리 도메인 네임과 함께 해당하는 IP 주소값을 한 쌍으로 저장하고 있는 일종의 데이터베이스 이다.
- 사용자가 웹 브라우저에 도메인 이름을 입력했을 때, 이름에 대한 IP 주소로 변환 요청을 **쿼리**라고 부른다.

## Domain Name Space

<aside>
💡 DNS가 저장, 관리하는 계층적 구조를 의미한다.

</aside>

- Domain은 계층 구조를 가지고 있다.
- 인터넷에서 사용되는 도메인은 IP 주소와 마찬가지로 전 세계적으로 고유하게 존재하여야 하므로 임의로 만들거나 변경할 수 없다.
    - 도메인 이름은 규칙을 따르고 있다.

![https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fbmht3K%2FbtrfcYhFCqu%2FPe3QTHhIiKBev9IJrTeRY1%2Fimg.png](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fbmht3K%2FbtrfcYhFCqu%2FPe3QTHhIiKBev9IJrTeRY1%2Fimg.png)

- 통상적으로 도메인의 가장 끝 점에 있는 점(.) 은 생략된다.
    - 이 점(.) 을 **Root 도메인**이라고 하며 가장 최상위 도메인이다.
- 최상위 도메인의 직속 하위 도메인을 **Top-level 도메인**이라고 부른다. (ex. com, net. co.kr)
- Top-level 도메인의 직속 하위 도메인을 **Second-level 도메인**이라고 부른다.
- Second-level 도메인의 직속 하위 도메인을 **Sub 도메인**이라고 부른다.
- 이러한 각각의 도메인을 **전담하는 독자적인 서버 컴퓨터가 존재**한다.

![https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FdE6y10%2FbtqOP4gTdWD%2FOvcpFElloTi1FFBFLteYbk%2Fimg.png](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FdE6y10%2FbtqOP4gTdWD%2FOvcpFElloTi1FFBFLteYbk%2Fimg.png)

## Name Server (DNS 서버)

- 도메인은 **도메인 계층 구조를 반영한 네임 서버에 저장, 관리**된다.
- 각 DNS 서버는 도메인 계층의 일부 영역을 담당하고 그 영역에 속한 도메인을 관리한다.
- 상위 계층 DNS 서버는 하위 계층의 도메인에 대한 정보를 관리하고 하위 계층 DNS 서버의 IP 주소를 가지고 있다.

![https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fm3SSL%2FbtqNZMM62oH%2FYqEKX3MBYyGVXry1Mu4vrK%2Fimg.png](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fm3SSL%2FbtqNZMM62oH%2FYqEKX3MBYyGVXry1Mu4vrK%2Fimg.png)

- Root Name Server는 하위 계층인 k, com의 정보를 관리하고 kr, com Name Server의 IP 주소가 등록되어 있다.

> DNS는 전 세계의 수많은 도메인을 효율적으로 저장하고 관리하기 위해서 **도메인을 계층화**하고, **계층의 일부 영역을 Name Server가 분산 관리**하는 시스템으로 설계, 운영되고 있다.
>

> 웹 브라우저에 도메인 입력시 가장 처음 PC 에 있는 Cache를 탐색한다. 없으면 DNS Server 질의를 시작한다.
>

## DNS Server 질의 과정

### 1. Local DNS Server (기지국 DNS 서버)

![https://www.wisewiredbooks.com/csbooks/ch3-network-internet/img/dns-iterated-step1.jpg](https://www.wisewiredbooks.com/csbooks/ch3-network-internet/img/dns-iterated-step1.jpg)

- DNS 쿼리 과정에서 해당 IP를 찾기 위해 가장 먼저 찾는 DNS 서버
- 기본적으로 컴퓨터 LAN 선을 통해 인터넷이 연결되면, 인터넷을 사용할 수 있게 IP를 할당해주는 통신사 (KT, SK, LG 등…) 에 해당되는 각 **통신사의 DNS 서버**가 등록된다.
- Local DNS Server에 찾는 도메인 주소가 있다면 IP 주소 반환한다.
- Local DNS Server에 찾는 도메인 주소가 없다면 Root DNS 서버에 물어본다.

### 2. Root DNS Server

![https://www.wisewiredbooks.com/csbooks/ch3-network-internet/img/dns-iterated-step2.jpg](https://www.wisewiredbooks.com/csbooks/ch3-network-internet/img/dns-iterated-step2.jpg)

- Local DNS Server에 해당 도메인에 대한 IP 주소가 없을 때, Local DNS Server는 Root DNS 서버에게 물어본다.
- Root DNS는 최상위 DNS 서버로 Root DNS부터 시작해서 Node DNS 서버에게로 차례차례 물어보게 됩니다.
- 전세계적으로 13개의 Root DNS Server가 존재한다.
    - ROOT 13대가 넘어가면 DNS 질의시 사용되는 메시지 사이즈가 512 바이트가 넘어가게 된다. (UDP 사용)
    - 애니캐스트 어드레싱을 사용하면 루트 서버 인스턴스의 실제 수를 더 크케 잡을 수 있으며 그 수는 1034 (2020.01.11 기준)

[Root Server Technical Operations Association](https://root-servers.org/)

### 3. Top-Level Domain DNS Server

![https://www.wisewiredbooks.com/csbooks/ch3-network-internet/img/dns-iterated-step3.jpg](https://www.wisewiredbooks.com/csbooks/ch3-network-internet/img/dns-iterated-step3.jpg)

- Root DNS Server에 해당 도메인 주소가 없다면 최상위 DNS Server에게 다시 물어본다.
    - Root DNS Server는 최상위 DNS 서버의 주소를 알려준다.
1. 국가 코드 최상위 도메인 (Country Code Top-Level Domain, ccTLD)
- ex) .kr, .jp, .CN, .US
1. 일반 최상위 도메인 (generic top-level domain, gTLD)
- ex) .com, .net, .org

### 4. 반복적 질의

![https://www.wisewiredbooks.com/csbooks/ch3-network-internet/img/dns-iterated-step4.jpg](https://www.wisewiredbooks.com/csbooks/ch3-network-internet/img/dns-iterated-step4.jpg)

- google.com의 네임서버 주소에 질의를 하면 google.com의 IP 주소를 응답해준다.
- TLD DNS Server에 IP 주소가 없다면 2차, 3차 도메인 Server에게 질의하면서 IP 주소를 찾을 때까지 반복한다.

### 5. DNS Cache 저장

- 위 과정을 통해 PC가 “google.com”의 IP주소를 성공적으로 받아왔다고 가정해보자.
- PC에는 DNS Cache라는 Cache를 활용해 Cache 안에 해당 Domain Name 주소를 저장한다. (운영체제의 DNS)
- TTL 이전에 다시 “google.com”에 다시 방문하기 위해 다시 접속을 시도한다면 DNS Server 쿼리는 일어나지 않고 Cache에서 IP 주소를 찾아 사용한다.
    - TTL - Time To Live
        - Cache에 저장되는 시간

## Reference

[https://hwan-shell.tistory.com/320](https://hwan-shell.tistory.com/320)

[https://better-together.tistory.com/128?category=887984](https://better-together.tistory.com/128?category=887984)

[https://www.wisewiredbooks.com/csbooks/ch3-network-internet/dns.html](https://www.wisewiredbooks.com/csbooks/ch3-network-internet/dns.html)