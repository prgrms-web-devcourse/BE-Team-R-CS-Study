# SQL Injection

## SQL Injection

<aside>
💡 데이터베이스에 전송되는 SQL 쿼리문을 조작하여 변조하거나, 허가 되지 않은 정보에 접근하는 공격

</aside>

- 주로 개인정보를 빼낼 때 많이 사용되는 기법이다.

![Untitled](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=http%3A%2F%2Fcfile21.uf.tistory.com%2Fimage%2F9920763C5C8890FB1AE43C)

## SQL Injection 종류 및 방법

### 1. Error based SQL Injection

<aside>
💡 논리적 에러를 이용한 SQL Injection

</aside>

- 논리적 에러를 이용한 SQL Injection은 가장 많이 쓰이고, 대중적인 공격 기법

![Untitled](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=http%3A%2F%2Fcfile22.uf.tistory.com%2Fimage%2F9958373C5C8890FA036E06)

- 일반적으로 로그인 시 많이 사용되는 SQL 구문
- 해당 구문에서 입력값에 대한 검증이 없음을 확인하고, 악의적인 사용자가 임의의 SQL 구문을 주입 (SQL Injection)
    - `' OR 1=1 --`
        - WHERE 절에 있는 싱글쿼터를 닫아주기 위한 싱글쿼터 `'`
        - `OR 1=1` 라는 구문을 이용해 WHERE 절을 모두 참으로 만든다.
        - `--`를 넣어줌으로 뒤의 구문을 모두 주석 처리
- 결론적으로 Users 테이블에 있는 모든 정보를 조회하게 됨으로 써 가장 먼저 만들어 진 계정으로 로그인에 성공하게 된다.
    - 보통 관리자 계정을 맨 처음 만들기 때문에 관리자 계정에 로그인 할 수 있다.
    - 관리자 계정을 탈취한 악의적인 사용자는 관리자의 권한을 이용해 또 다른 2차피해를 발생 시킬 수 있게 된다.

### 2. Union based SQL Injection

<aside>
💡 Union 명령어를 이용한 SQL Injection

</aside>

- Union 키워드는 두 개의 쿼리문에 대한 결과를 통합해서 하나의 테이블로 보여주게 하는 키워드

> Union Injection을 성공하기 위한 두 가지 조건
>
> - 컬럼의 개수가 같아야 한다.
> - 데이터 형이 같아야 한다.

![Untitled](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=http%3A%2F%2Fcfile4.uf.tistory.com%2Fimage%2F99BD4C3C5C8890FA0A2D9F)

- Board 라는 테이블에서 title과 contents 컬럼의 데이터랑 비교한 뒤 비슷한 글자가 있는 게시글을 검색하는 쿼리문
- Union 키워드와 함께 컬럼 수를 맞춰서 SELECT 구문을 넣어주게 되면 두 쿼리문이 합쳐서서 하나의 테이블로 보여진다.
    - 인젝션 한 구문은 사용자의 id와 password를 요청하는 쿼리문
        - id : long
        - password : String
        - null
    - 사용자의 개인정보가 게시글(board)과 함께 화면에 보여지게 된다.

### 3. Blind SQL Injection

<aside>
💡 데이터베이스로부터 특정한 값이나 데이터를 전달받지 않고, 단순히 참과 거짓의 정보만 알 수 있을 때 사용한다.

</aside>

### **Boolean based SQL**

- 로그인 폼에 SQL Injection이 가능하다고 가정 했을 때, 서버가 응답하는 **로그인 성공과 로그인 실패 메시지를 이용**하여, DB의 테이블 정보를 등을 추출해 낼 수 있다.

![Untitled](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=http%3A%2F%2Fcfile3.uf.tistory.com%2Fimage%2F99525F3C5C8890F90ED03D)

- Blind Injection을 이용해서 데이터베이스의 테이블 명을 알아내는 방법
- 악의적인 사용자는 `abc123’ and ASCII(SUBSTR(SELECT name From information_schema.tables WHERE table_type=’base table’ limit 0,1)1,1)) > 100 --` 구문 주입
    - limit 키워드를 통해 information_schema.tables에 있는 하나의 테이블만 조회 (0행 부터 1행까지)
    - SUBSTR 함수로 첫 글자만, 그리고 마지막 ASCII 를 통해서 ascii 값으로 변환
        - SUBSTR(str, pos, len) ⇒ 1번째 문자열부터 1글자만 가져오기
        - 조회되는 테이블 명이 Users 라면 ‘U’ 자가 ASCII 값으로 조회
    - 뒤의 100 이라는 숫자 값과 비교를 하게 된다.
    - 거짓이면 로그인 실패, 참이 될 때까지 뒤의 100이라는 숫자를 변경해 가면서 비교를 하게 된다.
        - 성공되는 숫자값을 알게된다 ⇒ 문자열의 ASCII 값을 알게된다.
- 공격자는 이 프로세스를 자동화 스크립트를 통해서 단기간 내에 **테이블 명**을 알아 낼 수 있다.

> **INFORMATION_SCHEMA ⇒ 테이블의 메타데이터**
- 테이블 메타데이터의 TABLES 및 TABLE_OPTIONS
- 열 및 필드 메타데이터의 COLUMNS 및 COLUMN_FIELD_PATHS
- 테이블 파티션 관련 메타데이터의 PARTITIONS
>

### **Time based SQL**

- **Sleep, Benchmark 가 되는 기준**으로 참과 거짓을 구별한다.

![Untitled](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=http%3A%2F%2Fcfile24.uf.tistory.com%2Fimage%2F99CAFB395C889145133A37)

- 테이터베이스의 길이를 알아내는 방법
- 악의적인 사용자가 `abc123’ OR (LENGTH(DATABASE())=1 AND SLEEP(2)) --` 구문 주입
    - LENGTH 함수는 문자열의 길이를 반환
    - DATABASE 함수는 데이터베이스의 이름을 반환
    - LENGTH(DATABASE()) = 1 가 참이면 SLEEP(2) 가 동작, 거짓이면 동작X
- 1 부분을 조작하여 **데이터베이스의 길이**를 알아 낼 수 있다.

## 대응방안

### 1. 입력 값에 대한 검증

<aside>
💡 서버 단에서 화이트리스트 기반으로 검증해야 한다.

</aside>

- SQL Injection 에서 사용되는 기법과 키워드는 엄청나게 많다.
- 블랙리스트 기반으로 검증하게 되면 수많은 차단리스트를 등록해야 하고, 하나라도 빠지면 공격에 성공하게 되기 때문

### 2. PreparedStatement 구문 사용

<aside>
💡 개발자가 먼저 모든 SQL 코드를 정의한 다음 나중에 각 매개 변수 전달하도록 한다.

</aside>

![Untitled](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTIh4U_0gkyPcwQznj5qKF9E15ngyPqt-36kdQ48PWhR4ohw7aRFDHURYGxKCA_uf2p5g&usqp=CAU)

- DBMS 내부적으로 4가지 과정 (parse, bind, execute, fetch)을 거쳐 결과를 출력
- 문법을 검사하기 위해서 parse 과정을 거치게 된다.

![Untitled](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTnIdpC0DoNuw7M7IQ_3krBPnEjgRslNDot2A&usqp=CAU)

- 일반적인 Statement를 사용하여 SELECT 쿼리를 입력했을 때에는 매법 parse부터 fetch까지 모든 과정을 수행한다.
- Prepared Statement를 사용하는 경우에는 효율을 높이기 위해 parse 과정을 최초 1번만 수행
    - 자주 변경되는 부분을 변수로 선언해 두고, 매번 다른 값을 대입하여 사용한다.
- 그 후 사용자의 입력 값을 문자열로 인식하게 하여 공격 쿼리가 들어간다고 하더라도, 사용자의 입력은 이미 의미 없는 **단순 문자열** 이기 때문에 전체 쿼리문도 공격자의 의도대로 작동하지 않는다.

### 3. Error Message 노출 금지

<aside>
💡 데이터베이스에 대한 오류 발생 시 사용자에게 보여줄 수 있는 페이즈를 제작 혹은 메시지박스를 띄우도록 하여야 한다.

</aside>

- 공격자가 SQL Injection을 수행하기 위해서는 데이터베이스의 정보(테이블명, 컬럼명 등)가 필요하다.
- 데이터베이스 에러 발생 시 따로 처리를 해주지 않았다면, 에러가 발생한 쿼리문과 함께 에러에 관한 내용을 반환해 준다.
    - 여기서 테이블명 및 컬럼명을 그리고 쿼리문이 노출이 될 수 있다.

## 예상 질문

### 1. SQL Injection 설명

### 2. SQL Injection 공격 방어 기법

## 참고문헌

[https://noirstar.tistory.com/264](https://noirstar.tistory.com/264)

[https://github.com/gyoogle/tech-interview-for-developer/blob/master/Computer Science/Database/SQL Injection.md](https://github.com/gyoogle/tech-interview-for-developer/blob/master/Computer%20Science/Database/SQL%20Injection.md)