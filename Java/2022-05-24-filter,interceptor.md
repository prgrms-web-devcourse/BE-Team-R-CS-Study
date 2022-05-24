
***Q . Filter와 Interceptor의 비교(차이점, 공통점)***


<br>


어떤 작업의 앞, 중간, 뒤의 ***공통 업무***(웹 통신과 관련한)들를 분리해서 처리할 수 있는 방법

AOP와 엮이기도 함.

(웹 통신을 기준으로 하는 게 아니라, 비즈니스 로직에 대한 중복 업무에 대하여 동작, ex : @Transactional)




![image](https://user-images.githubusercontent.com/72663337/170065702-cdd8a7e2-fa72-441b-b52c-da22744efc64.png)


`(https://justforchangesake.wordpress.com/2014/05/07/spring-mvc-request-life-cycle/)`



<br>

## Filter

Dispatcher Servlet(Front controller) 의 앞단에서 정보를 처리(Spring 범위 밖에서 처리, Spring Context 외부)

보통은 URL 패턴에 따라 실행됨.(CORS filter, Spring Security) 



- 인코딩 변환처리, XSS 방어, 인증(Spring Security), CORS 등을 구현할 때 사용
- Servlet에서 처리하기 전후를 다룸(Servlet Request, Response를 조작할 수 있음) -> doFilter method
- Filter chain

<br>


Filter interface 를 implements 하여 구현


```
public interface Filter {
    default void init(javax.servlet.FilterConfig filterConfig) throws javax.servlet.ServletException { /* compiled code */ }

    void doFilter(javax.servlet.ServletRequest servletRequest, javax.servlet.ServletResponse servletResponse, javax.servlet.FilterChain filterChain) throws java.io.IOException, javax.servlet.ServletException;

    default void destroy() { /* compiled code */ }
}
```

<br>


Spring Security 에서의 Filter 적용과 Filter chain 예시

(웹 요청을 가로채어 사용자에 대한 인증, 권한 소유 확인)

![image](https://user-images.githubusercontent.com/72663337/170066497-762f62af-ac2e-4c23-9057-a276627fd992.png)

`Filter Chain`


```
//debug true 어노테이션을 통하여 실행되는 Security Filter들을 확인
@EnableWebSecurity(debug=true)
public class WebSecurityConfig extends WebSecurityConfigurerAdapter {

    @Override
    protected void configure(HttpSecurity http) throws Exception {
		http
        ...

```




![image](https://user-images.githubusercontent.com/72663337/170066658-3e92b187-31d5-43e4-81d9-b24ec46536fd.png)





`Spring Security의 Filter chain`


<br>



## Interceptor
요청에 대한 ***작업 전/후***에서 처리

- Spring MVC에서 제공하는 API(Spring context)
- Dispatcher servlet이 controller를 호출하기 전에 작동
- 세션에 대한 체크, 인증 처리
- preHandle, postHandle,(handler를 실행하기 전,후) afterCompletion(view 랜더링한 후) 등 메소드에 따라 실행 시점을 다르게 가져갈 수 있음


![image](https://user-images.githubusercontent.com/72663337/170066825-f20d1f5f-f827-4f6a-a44d-f91c29f7caff.png)



`Interceptor flow(https://velog.io/@y_dragonrise/Spring-Interceptor-%EA%B0%9C%EB%85%90-%EB%B0%8F-%ED%9D%90%EB%A6%84)`


HandlerInterceptor implements 하여 구현


```
public interface HandlerInterceptor {
    default boolean preHandle(javax.servlet.http.HttpServletRequest request, javax.servlet.http.HttpServletResponse response, java.lang.Object handler) throws java.lang.Exception { /* compiled code */ }

    default void postHandle(javax.servlet.http.HttpServletRequest request, javax.servlet.http.HttpServletResponse response, java.lang.Object handler, @org.springframework.lang.Nullable org.springframework.web.servlet.ModelAndView modelAndView) throws java.lang.Exception { /* compiled code */ }

    default void afterCompletion(javax.servlet.http.HttpServletRequest request, javax.servlet.http.HttpServletResponse response, java.lang.Object handler, @org.springframework.lang.Nullable java.lang.Exception ex) throws java.lang.Exception { /* compiled code */ }
}

```



 +

Dispatcher Servlet 내에서 작동하기 때문에 ControllerAdvice, ExceptionHandler등으로 예외 처리가 가능함.






<br>


#### 공통점
- 웹 통신과 연관된 ***공통 작업***을 분리하여 처리하는 역할을 가짐.




#### 차이점

| 	|filter|interceptor|
|:---:|:---:|:---:|
|실행 시점	|Dispatcher Servlet에 Request가 들어가기 전|	Controller를 호출하기 전|
|등록 Container	|Web Container	|Spring Container|
|Requeset, Response 조작|	가능|	불가능|








<br>

### Reference



https://velog.io/@y_dragonrise/Spring-Interceptor-%EA%B0%9C%EB%85%90-%EB%B0%8F-%ED%9D%90%EB%A6%84


https://supawer0728.github.io/2018/04/04/spring-filter-interceptor/


https://baek-kim-dev.site/61


https://junhyunny.github.io/spring-boot/filter-interceptor-and-aop/

