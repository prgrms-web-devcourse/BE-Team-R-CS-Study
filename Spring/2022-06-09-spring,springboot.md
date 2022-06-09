### Spring(Spring framework)

> The Spring Framework provides a comprehensive programming and configuration model for modern Java-based enterprise applications - on any kind of deployment platform.


- 스프링 프레임워크는 자바 기반의 어플리케이션을 위한 프로그래밍을 제공한다.


  - 객체 지향의 특징을 잘 활용할 수 있게 해주고, 개발자로 하여금 비즈니스 로직 구현에만 집중할 수 있게 하여 생산성을 높임

  - (DI, IoC)


<br>


### SpringBoot
>Spring Boot makes it easy to create stand-alone, production-grade ***Spring based Applications*** that you can "just run".
We take an opinionated view of the Spring platform and third-party libraries so you can get started with minimum fuss. Most Spring Boot applications need ***minimal Spring configuration.***


 - 스프링 기반의 어플리케이션을 쉽게 만들 수 있게 해 주는 프로젝트

 - 약간의 설정(minimal Spring configuration)만으로도 어플리케이션을 제작할 수 있게 해 줌.



![image](https://user-images.githubusercontent.com/72663337/172805496-5f74c87b-3178-41b8-bb71-8a16415028b6.png)



   - => SpringBoot는 Spring framework를 쉽게 사용할 수 있게 해주는 도구. 



<br>



### SpringBoot 사용 시 달라지는 점

#### 1. 의존성 관리


**Spring**

- 여러 의존성들을 추가할 때 필요한 의존성에 대한 호환되는 버전을 알고 있어야 하고, 개발자가 이를 직접 명시해 주어야 함.

```
<dependency>
  <groupId>org.hamcrest</groupId>
  <artifactId>hamcrest</artifactId>
  <version>2.2</version>
  <scope>compile</scope>
</dependency>
<dependency>
  <groupId>org.junit.jupiter</groupId>
  <artifactId>junit-jupiter</artifactId>
  <version>5.8.2</version>
  <scope>compile</scope>
</dependency>
<dependency>
  <groupId>org.mockito</groupId>
  <artifactId>mockito-core</artifactId>
  <version>4.5.1</version>
  <scope>compile</scope>
</dependency>
..
```

- test를 위하여 필요한 의존성들을 추가하는 경우(jupiter, hamcrest...)

위와 같이 의존성 버전을 맞추어 설정해 주어야 함!!



<br>



***SpringBoot***

```
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-test</artifactId>
    <scope>test</scope>
</dependency>
```

 - 자동으로 다수의 의존성이 정의되었기 때문에 간단하게 설정을 끝낼 수 있다. (ex : spring-boot-starter )

 - spring-boot-starter-test 의존성 추가를 통해 위의 의존성들을 대체할 수 있음



![image](https://user-images.githubusercontent.com/72663337/172805621-308a62d0-d85d-4957-a793-bebfe62090ad.png)


- spring-boot-starter-test 내부(jupitor, mokito 등의 라이브러리들을 내장하고 있음을 description에서부터도 알 수 있다.)


<br>


#### 2. 설정의 자동화


***Spring***

 - configuration 파일(설정값들을 정의하는 파일)에 bean 들을 명시적으로 작성하는 방식으로 등록시켜주어야 함.

		...
        @Bean
        public DataSource dataSource() {

            var dataSource = DataSourceBuilder.create()
                    .url("jdbc:mysql://localhost:2215/test-voucher_prgrms")
                    .username("test")
                    .password("test123")
                    .type(HikariDataSource.class)
                    .build();

            return dataSource;
        }

        @Bean
        public JdbcTemplate jdbcTemplate(DataSource dataSource) {
            return new JdbcTemplate(dataSource);
        }
        ...
 - 데이터베이스와의 연결을 위해 빈들을 등록해주어야 함





***SpringBoot***

- AutoConfiguration 기능으로 위를 대체할 수 있음



![image](https://user-images.githubusercontent.com/72663337/172806194-bce44f5d-80e7-4226-a394-4586ceb039a2.png)


 - 부트 프로젝트에서 사용되는 @SpringBootApplication



![image](https://user-images.githubusercontent.com/72663337/172806308-35be8c20-483b-4970-b016-a6bc117cf94f.png)


@SpringBootApplication 어노테이션 안에 설정을 자동으로 세팅해주는 여러 어노테이션이 동작하고 있음.


자동 설정을 도와주는 어노테이션을 보면

***@ComponentScan***

- 패키지들을 스캔하면서 Bean으로 등록할 클래스들을 탐색/등록

***@EnableAutoConfiguration***

- 마찬가지로 Spring.factories라는 설정 파일을 통하여 설정값들에 따라 추가적으로 Bean 등록



위의 어노테이션에 의해 프로젝트에서 요구되는 Bean들이 생성/등록되며, 설정 파일에 명시해주지 않아도 자동 설정이 이루어지게 됨.


<br>


#### 3. 내장 WAS(Web Application Server)

***Spring***

프로젝트를 war(Web application ARchive) 파일로 패키징 -> WAS 설치 -> 설치한 WAS 서버에 WAR 파일을 올려 배포





***SpringBoot***

내장된 WAS 서버를 갖고 있기 때문에 위와 같은 과정은 필요 없음

Run 버튼을 누르는 것만으로도 서버 위에서 프로젝트를 동작시킬 수 있음




<br>


#### 4. 모니터링
- 실행 중 어플리케이션의 상태를 모니터링, 관리할 수 있는 기능

- Actuator(CPU, JVM memory 사용률, 로그, endpoint에서의 작업 관리 등) 를 통하여 모니터링 과정이 이루어 짐.

   (민감 정보를 제공할 수 있기 때문에 운영 시에는 spring security를 사용하는 방법 등을 통해 보안에 신경을 써주어야 함.)





<br>

### 결론

SpringBoot는 Spring framework를 통한 프로젝트에 대하여 자동 설정을 쉽게 해주는 프로젝트.

개발자가 설정 세팅과 같은 문제에 소모하는 비용을 줄이게 함.



<br>

### Reference

https://spring.io/projects/spring-framework


http://theeye.pe.kr/archives/2014


https://www.youtube.com/watch?v=6h9qmKWK6Io




