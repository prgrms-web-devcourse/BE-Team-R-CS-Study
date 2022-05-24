# Web Server, WAS, Servlet

## 1. Web Server란?

### (1) Web Server의 개념

- 하드웨어 : Web 서버가 설치되어 있는 컴퓨터
- 소프트웨어 : 웹 브라우저 클라이언트로부터 HTTP 요청을 받아 정적인 컨텐츠(.html .jpeg .css 등)를 제공하는 컴퓨터 프로그램
- Ex) Apache Server, Nginx, IIS(Windows 전용 Web 서버) 등

<br/>

### (2) Web Server의 기능

- HTTP 프로토콜을 기반으로 하여 클라이언트(웹 브라우저 또는 웹 크롤러)의 요청을 서비스 하는 기능을 담당한다.
- 요청에 따라 아래의 두 가지 기능 중 적절하게 선택하여 수행한다.
1. 정적인 컨텐츠 제공 : WAS를 거치지 않고 바로 자원을 제공한다.
2. 동적인 컨텐츠 제공을 위한 요청 전달 : 클라이언트의 요청(Request)을 WAS에 보내고, WAS가 처리한 결과를 클라이언트에게 전달(응답, Response)한다.

<br/>

### (3) Web Server가 필요한 이유?

클라이언트한테 이미지 파일을 포함한 HTML을 보내는 과정을 생각해보자.

이미지 파일과 같은 정적인 파일들은 HTML이 클라이언트한테 보내질 때, 함께 가는 것이 아니다.

클라이언트는 HTML 문서를 먼저 받고, 그에 맞게 필요한 이미지 파일들을 다시 서버한테 요청하면, 그제서야 서버는 해당하는 이미지 파일들을 보내준다.

따라서, Web Server는 요청을 WAS까지 보내지 않고 정적인 파일들을 앞단에서 빠르게 보내준다.

<br/>

## 2. WAS (Web Application Server)란?

![Untitled](https://user-images.githubusercontent.com/50768959/169973892-574352e7-9b81-47a3-bab6-42712fe9a921.png)

출처 : [https://gmlwjd9405.github.io/2018/10/27/webserver-vs-was.html](https://gmlwjd9405.github.io/2018/10/27/webserver-vs-was.html)

### (1) WAS의 개념

WAS는 동적인 컨텐츠를 제공하기 위해 만들어진 Application Server로 웹 컨테이너 또는 서블릿 컨테이너라고도 불린다.

서블릿 컨테이너는 서블릿 객체를 자동으로 생성 및 호출하고, 서블릿 컨테이너가 종료될 때, 서블릿 객체들 또한 종료시켜준다.

즉, 서블릿 컨테이너는 서블릿의 생명 주기를 관리한다.

Ex) Tomcat, JBoss, Jeus 등

<br/>

### (2) WAS의 역할

- WAS = Web Server + Web Container

주로 프로그램 실행 환경과 DB 접속 기능을 제공하고, 비즈니스 로직을 수행한다. 또한 트랜잭션 관리 기능도 제공한다.

<br/>

### (3) WAS가 필요한 이유?

웹 페이지에는 정적 컨텐츠와 동적 컨텐츠가 모두 존재한다.

이 때, Web Server만을 이용한다면 사용자가 원하는 요청에 대한 결과값을 모두 미리 다 만들어 놓고 서비스를 해야한다. 하지만 이렇게 수행하기에는 자원이 절대적으로 부족하다.

따라서 WAS를 통해 요청에 맞는 비즈니스 로직을 실행 후, 그때 그때 결과를 동적으로 만들어서 제공함으로써 자원을 효율적으로 사용할 수 있다.

<br/>

## 3. Web Server와 WAS를 구분하는 이유?

![Untitled 1](https://user-images.githubusercontent.com/50768959/169973929-e1661acc-c476-4406-b686-b45946db5684.png)

출처 : [https://gmlwjd9405.github.io/2018/10/27/webserver-vs-was.html](https://gmlwjd9405.github.io/2018/10/27/webserver-vs-was.html)

WAS는 정적 컨텐츠, 동적 컨텐츠 모두 제공할 수 있으니까, WAS가 Web Server의 기능까지 모두 수행하면 되지 않을까?

왜 굳이 Web Server와 WAS를 구분해서 사용하는걸까?

그 이유에는 다음과 같은 것들이 있다.

<br/>

### (1) 기능을 분리하여 서버 부하를 방지한다.

WAS는 DB 조회 등 비즈니스 로직을 처리하느라 바쁘다.

만약에 정적 컨텐츠 요청까지 WAS가 처리한다면 서버 부하가 커지게 되고, 동적 컨텐츠의 처리가 지연됨에 따라 빨리 처리할 수 있는 정적 컨텐츠 처리까지 지연되게 된다.

이로 인해 페이지 노출 시간까지 늘어나게 된다.

<br/>

### (2) 물리적으로 분리하여 보안 강화

SSL에 대한 암복호화 처리에 Web Server를 사용한다. 

<br/>

### (3) 여러 대의 WAS를 연결 가능

Web Server는 Load Balancing을 위해 사용할 수도 있는데, 이 때, fail over(장애 극복), fail back 처리에 유리하다.

특히 대용량 웹 어플리케이션의 경우 여러 개의 서버를 사용하게 되는데, Web Server와 WAS를 분리하여 무중단 운영을 위한 장애 극복에 쉽게 대응할 수 있다.

예를 들어, 앞 단의 Web Server에서 오류가 발생한 WAS를 이용하지 못하도록 한 후, WAS를 재시작함으로써 사용자는 오류를 느끼지 못하고 이용할 수 있다.

**즉, 자원 이용의 효율성 및 장애 극복, 배포 및 유지 보수의 편의성을 위해 Web Server와 WAS를 분리한다.**

**Web Server를 WAS 앞에 두고 필요한 WAS를 Web Server에 플러그인 형태로 설정하면 더욱 효율적인 분산 처리가 가능하다.**

<br/>

## 4. Servlet이란?

Servlet에 대한 정의를 위키백과에서 찾아보면 다음과 같다.


**⭐ 자바 서블릿(Java Servlet)은 자바를 사용하여 웹페이지를 동적으로 생성하는 서버측 프로그램 혹은 그 사양을 말한다.**


Servlet의 역할을 좀 더 정리해보자면 다음과 같다.

- 클라이언트의 요청에 따라 적절한 Controller에게 요청 전달
- 비즈니스 프로세스를 제외한 WAS에서 하는 기본적인 요청, 응답 제어에 관련된 처리

<br/>

### (1) Servlet의 동작 방식
![Untitled 2](https://user-images.githubusercontent.com/50768959/169973954-ccff1a68-4e77-4c41-892a-405833ea593f.png)


출처 : [https://gmlwjd9405.github.io/2018/10/27/webserver-vs-was.html](https://gmlwjd9405.github.io/2018/10/27/webserver-vs-was.html)

1. Web Server는 웹 브라우저 클라이언트로부터 HTTP 요청을 받는다.
2. Web Server는 클라이언트의 요청(Request)을 WAS에 보낸다.
3. WAS는 관련된 Servlet을 메모리에 올린다.
4. WAS는 web.xml을 참조하여 해당 Servlet에 대한 Thread를 생성한다.
    - 매번 Thread를 생성하게 되면 속도도 늦어지고 생성 비용도 많이 들게 되고, 쓰레드 생성에 제한을 걸 수 없어서 서버가 죽을 수 있다.
    - 따라서, WAS는 쓰레드 풀을 사용한다.
        
        → 쓰레드가 미리 생성되어 있어 속도 및 생성 비용 면에서 장점이 있고, 쓰레드 개수에 제한을 걸 수 있기 때문에 안전한 처리가 가능하다.
        
5. HttpServletRequest와 HttpServletResponse 객체를 생성하여 Servlet에 전달한다.
    - Thread는 Servlet의 service() 메서드를 호출한다.
    - service() 메서드는 요청에 맞게 doGet() 또는 doPost() 메서드를 호출한다.
6. doGet() 또는 doPost() 메서드는 인자에 맞게 생성된 적절한 동적 페이지를 Response 객체에 담아 WAS에 전달한다.
7. WAS는 Response 객체를 HttpResponse 형태로 바꾸어 Web Server에 전달한다.
8. 생성된 Thread를 종료하고, HttpServletRequest와 HttpServletResponse 객체를 제거한다.

<br/>

### (2) Servlet 객체

만약 Client한테서 요청이 올 때 마다 계속 Serlvet 객체를 생성한다면 굉장히 비효율적일것이다.

따라서 최초 로딩 시점에 Servlet 객체를 미리 만들어두고 재활용을 하고, 해당 Servlet 객체는 싱글톤으로 관리가 된다. 즉, 모든 Client 요청을 같은 서블릿 객체를 사용하개 된다.

→ 공유 변수, 상태를 관리하지 않도록 주의해야 한다.

<br/>

### (3) Servlet의 생명 주기

1. init : Servlet Instance 생성
    - 클라이언트의 요청이 들어오면 컨테이너는 해당 서블릿이 메모리에 있는지 확인하고, 없을 경우 init()메소드를 호출하여 적재한다.
    - init()메소드는 처음 한번만 실행되기 때문에, 서블릿의 쓰레드에서 공통적으로 사용해야하는 것이 있다면 오버라이딩하여 구현하면 된다.
    - 실행 중 서블릿이 변경될 경우, 기존 서블릿을 파괴하고 init()을 통해 새로운 내용을 다시 메모리에 적재한다
2. Service : 실제 기능 수행
    - 클라이언트의 요청에 따라서 service()메소드를 통해 요청에 대한 응답이 doGet()가 doPost()로 분기됩니다.
    - 이 때, 가장 먼저 처리하는 과정으로 생성된 HttpServletRequest, HttpServletResponse에 의해 request와 response객체가 제공됩니다.
3. Destroy : Servlet Instance 삭제
    - 컨테이너가 서블릿에 종료 요청을 하면 destroy()메소드가 호출된다.
    - 종료시에 처리해야하는 작업들은 destroy()메소드를 오버라이딩하여 구현하면 된다.
    - 보통 서블릿 컨테이너가 종료되는 시점에 호출하며, 특정 servlet 로드/언로드 시에도 사용된다.
