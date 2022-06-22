# Web Socket

## 웹 소켓이란?

<aside>
💡 HTML5 표준 기술로, HTTP 환경에서 클라이언트와 서버 사이에 하나의 TCP 연결을 통해 **실시간으로 전이중 통신을 가능하게 하는 프로토콜**

</aside>

- **실시간 알림, 실시간 채팅** 등 실시간이라는 키워드가 들어가는 기능들을 위해서는 대부분 웹 소켓 기술이 필요하다.

## HTTP VS 웹소켓

### HTTP

- 비연결성 (connectionless)
    - HTTP는 클라이언트와 서버간 접속을 유지하지 않는다.
- 단방향 통신
    - 서버에서 클라이언트로의 요청이 불가능하다.

### 웹소켓

- 양방향 통신
    - 데이터 송수신을 동시에 할 수 있다.
    - Client와 Server가 서로 원할 때 데이터를 주고 받는다.
- 연결지향

> 초기 웹의 탄생 목적은 문서 전달과 하이퍼링크를 통한 문서 연결이였다.
웹을 위한 HTTP 프로토콜은 실시간 채팅과 같은 기능에 매우 부적합한 모델이다.
>

## HTTP 실시간성 기술

HTTP를 이용해서 실시간성을 구현할 수 있는 기술.

### (1) Polling

- 서버로 **주기적으로 요청하여 결과를 확인**하는 방식
    - 값이 없더라도 빈 값을 응답 받는다.
- 요청 주기 짧으면 불필요한 요청/응답 트래픽이 많이 발생한다.
- 요청에 대한 서부 부담이 크지 않거나 실시간 메시지 전달이 크게 중요하지 않는 서비스에 적합

![https://d2.naver.com/content/images/2015/06/helloworld-1052-2.png](https://d2.naver.com/content/images/2015/06/helloworld-1052-2.png)

### (2) Long Polling

- 요청에 대한 응답을 **서버 이벤트 발생 시점에 받는 방식**
- polling 방식에 비해 불필요한 트래픽을 덜 유발한다.
- 실시간 메시지 전달이 중요하지만 서버의 상태 변경이 빈번하지 않는 서비스에 적합

![https://d2.naver.com/content/images/2015/06/helloworld-1052-3.png](https://d2.naver.com/content/images/2015/06/helloworld-1052-3.png)

### (3) HTTP Streaming

- 서버에 요청 보내고 끊기지 않은 연결상태에서 끊임없이 데이터 수신
- 응답마다 다시 요청해야 하는 Long Polling에 비해 효율적이며, 서버의 상태 변경이 잦은 경우에 유리하다.
- 연결을 길게 맺고 있는 경우 유효성 관리 등의 부담이 발생한다.
    - 보통 특정 시간을 주기로 연결을 재설정하도록 구현하는 방식으로 해결

![https://d2.naver.com/content/images/2015/06/helloworld-1052-4.png](https://d2.naver.com/content/images/2015/06/helloworld-1052-4.png)

> 세가지 방법 다 HTTP를 통신하기에 **Request와 Response 둘다 Header가 불필요하게 크다.** (실시간 채팅에는 부적합)
>
> - Protocol에 대한 스펙의 변화가 필요하다.

## 웹 소켓의 동작 방식

![https://github.com/NKLCWDT/cs/raw/main/images/websocket.png](https://github.com/NKLCWDT/cs/raw/main/images/websocket.png)

### 1. Opening Handshake

- 웹 소켓은 전이중 통신이므로, **연속적인 데이터 전송의 신뢰성을 보장**하기 위해 Handshake 과정을 진행한다.
- 기존의 다른 TCP 기반의 프로토콜은 TCP layer에서의 Handshake를 통해 연결을 수립하는 반면, 웹 소켓은 **HTTP 요청 기반으로 Handshake 과정**을 거쳐 연결을 수립한다.

(1) 웹 소켓은 연결을 수립하기 위해 `Upgrade 헤더` 와 `Connection 헤더` 를 포함하는 HTTP 요청을 보낸다.

![https://user-images.githubusercontent.com/50273712/129447412-30e32809-b1fe-4e95-9d7a-85553f3ab92b.png](https://user-images.githubusercontent.com/50273712/129447412-30e32809-b1fe-4e95-9d7a-85553f3ab92b.png)

- **Upgrade**
    - 현재 protocol에서 다른 protocol로 업그레이드 또는 변경 할때 사용한다.  (websocket으로 변경)
    - 이 값이 없거나 다른 값이면 cross-protocol attack 이라고 간주하여 웹 소켓 접속을 중지시킨다.
- **Connection**
    - 현재의 전송 완료 후 network를 유지할 것인지에 대한 정보
    - upgrade 값이 없거나 다른 값이면 웹 소켓 연결 중지
- Sec-WebSocket-Key
    - 유효한 요청인지 확인하기 위해 사용하는 키 값
- Sec-webSocket-Protocol
    - 사용하고자 하는 하나 이상의 웹 소켓 프로토콜 지정
- Origin
    - 클라이언트의 주소

(2) 통상적인 상태 코드 200 대신, **웹 소켓 서버의 응답**은 다음과 같다.

![https://user-images.githubusercontent.com/50273712/129448362-0ada9130-1181-4d3b-ac4b-33bd4130b00d.png](https://user-images.githubusercontent.com/50273712/129448362-0ada9130-1181-4d3b-ac4b-33bd4130b00d.png)

- 101 Switching Protocols
    - “프로토콜 전환”을 서버가 승인했음을 알리는 코드
- Upgrade와 Connection은 동일하게 넣어주어야 한다.
- Sec-WebSocket-Accept
    - 클라이언트로부터 받은 Sec-WebSocket-Key를 사용하여 계산된 값
    - 클라이언트에서 계산한 값과 일치하지 않으면 연결 수립하지 않는다.

<aside>
💡 Handshake 과정을 통해 연결이 수립되면 응용 프로그램 계층 프로토콜이 HTTP에서 웹 소켓으로 업그레이드가 된다.
업그레이드가 되면 HTTP는 사용되지 않고, 웹 소켓 연결이 닫힐 때까지 두 끝 점에서 **웹 소켓 프로토콜을 사용**하여 데이터를 주고 받게 된다.

</aside>

### 2. Data transfer

- 웹소켓 연결수립이 되면 데이터 전송이 시작된다.
- Client와 Server가 message라는 개념으로 데이터 주고 받는다.
    - 프레임으로 구성된 메시지라는 논리적 단위로 송수신

### 3. Closing Handshake

- 클라이언트나 서버가 커넥션 종료를 위한 컨트롤 프레임 전송
- 웹 소켓 연결은 주로 새로고침이나 창 닫기 등의 이벤트 발생 시 닫힌다.

## 웹 소켓 프로토콜 특징

- 최초 접속에서만 HTTP 프로토콜 위에서 handshaking을 하기 때문에 HTTP header를 사용한다.
- 웹소켓을 위한 별도의 포트는 없으면, 기존 포트(http-80, https-443)을 사용
- 프레임으로 구성된 메시지라는 논리적 단위로 송수신
- **메시지에 포함될 수 있는 교환 가능한 메시지는 텍스트와 바이너리**

## 웹소켓 한계

- 웹소켓은 HTML5 표준 기술이다.
    - HTML5 이전의 기술로 구현된 서비스에서 웹 소켓처럼 사용할 수 있도록 도와주는 기술
        - Socket.io, SockJS
    - 브라우저와 웹 서버의 종류와 버전을 파악하여 가장 적합한 기술을 선택하여 사용하는 방식
- WebSocket은 문자열과 바이너리 두 가지 유형의 메시지 타입만 받는다.
    - 메세지의 내용에 대해서는 정의하지 않는다.
        - 해당 메시지가 어떤 요청인지
        - 어떤 포맷으로 오는지
        - 메시지 통신 과정을 어떻게 처리해야 하는지
    - 때문에 WebSocket방식은 sub-protocols을 사용해서 주고 받는 메시지의 형태를 약속하는 경우가 많다.
        - STOMP

### Reference

[https://github.com/NKLCWDT/cs/blob/main/Network/웹소켓.md](https://github.com/NKLCWDT/cs/blob/main/Network/%EC%9B%B9%EC%86%8C%EC%BC%93.md)

[https://tecoble.techcourse.co.kr/post/2021-08-14-web-socket/](https://tecoble.techcourse.co.kr/post/2021-08-14-web-socket/)