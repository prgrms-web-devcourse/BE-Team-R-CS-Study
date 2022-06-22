# Redirect & Forward

## 리다이렉트 (Redirect)

<aside>
💡 클라이언트에서 요청한 URL의 서버의 응답으로 다른 URL로 재접속 명령을 보내는 것

</aside>

- 리다이렉트가 일어나면 URL 주소가 바뀌면서 다시 접속되는 것을 확인할 수 있다.

![https://mdn.mozillademos.org/files/13785/HTTPRedirect.png](https://mdn.mozillademos.org/files/13785/HTTPRedirect.png)

- Client가 Server에 Resource를 요청한다.
- Server는 **HTTP 응답 상태코드 3XX**와 함께 **Location 헤더에 Redirect 주소**를 받아서 보낸다.
- Client는 새로운 주소값으로 다시 Resource를 요청한다.
    - 클라이언트는 HTTP 응답 상태코드 3XX를 통해 리다이렉트를 인지하고 Location 헤더를 통해서 Redirect 경로로 재요청한다.
- Server는 새로운 Resource를 응답한다.

### Redirection & HTTP Response Status Code

- HTTP Response Status Code는 요청에 대한 웹서버의 응답을 나타내는 코드를 말한다.
- 3XX 모두 사용자를 새로운 URL로 이동시키는 코드이다.

**1. 영구 리다이렉션 (301, 308)**

- 리소스의 URI가 영구적으로 이동
    - ex) /members → /users
- 원래의 URL을 사용하지X
- 301 (Moved permanently)
    - 리다이렉트시 요청 메서드가 GET으로 변하고, 본문이 제거될 수 있다.
- 308 (Permanent Redirect)
    - 리다이렉트시 요청 메서드와 본문 유지 (처음 POST를 보내면 리다이렉트도 POST 유지)

**2. 일시적인 리다이렉션 (302, 307, 303)**

- 리소스의 URI가 일시적으로 변경
    - 주문 완료 후 주문 내역 화면으로 이동 (Post, Redirect, Get)
- 302 (Found)
    - 리다이렉트시 요청 메서드가 GET으로 변하고, 본문이 제거될 수 있다.
- 307 (Temporary Redirect)
    - 리다이렉트시 요청 메서드와 본문 유지
- 303 (See Other)
    - 리다이렉트시 요청 메서드가 Get으로 변경

## Forward

<aside>
💡 클라이언트의 URL 요청이 Server 내부에서 다른 URL 호출된다.

</aside>

- 서버에서 포워딩된 URL의 리소스를 확인하여 클라이언트에 응답한다.
- 요청된 URL의 변화가 없다.
    - 클라이언트 입장에서는 Forward가 일어났는지 모른다.

![https://nesoy.github.io/assets/posts/20180409/2.png](https://nesoy.github.io/assets/posts/20180409/2.png)

- Client가 Server에 Resource를 요청합니다.
- Server는 Web Container에 의해  LoginServlet에서 HelloServlet로 forward하게 된다.
- Server는 최종적으로 Hello의 결과를 응답하게 된다.
- Client는 한번의 요청으로 결과를 받을 수 있다.

> Forward는 Web Contrainer의 내부에서 이동하기 때문에 request와 response 객체를 공유할 수 있다.  (내부에서 자원을 공유)
>

## Spring에서의 Redirect & Forward

### 1. Redirect

- `String`
    - 접두사 “redirect:”와 함께 반환 되면 `UrlBasedViewResolver` 는 이를 리다이렉션이 발생해야 한다는 특별한 표시로 인식한다.
- `ModelAndView`
- `httpServletResponse.sendRedirect()`
- `RedirectView`
- `ResponseEntity`

```java
@Controller
public class RedirectController {

    @GetMapping("/redirect1")
    public String redirect1() {
        return "redirect:https://www.naver.com";
    }

    @GetMapping("/redirect2")
    public ModelAndView redirect2() {
        return new ModelAndView("redirect:https://www.naver.com");
    }

    @GetMapping("/redirect3")
    public RedirectView redirect3() {
        RedirectView redirectView = new RedirectView("https://www.naver.com");
        return redirectView;
    }

    @GetMapping("/redirect4")
    public ResponseEntity<Void> redirect4() {
        return ResponseEntity.status(HttpStatus.FOUND).location(URI.create("https://www.naver.com")).build();
    }

    @GetMapping("/redirect5")
    public void redirect5(HttpServletResponse httpServletResponse) throws IOException {
        httpServletResponse.sendRedirect("https://www.naver.com");
    }

}
```

- @Controller 그리고 @RestController에서 모두 사용가능한 방식이다.
    - String 방식 제외
    - String 방식은 바로 json 문자열로 직렬화되어 Return 되어진다.

> @Controller VS @RestController
- @Controller
  String, ModelAndView Return 타입인 경우 해당 문자열 ViewName을 가지는 html을 찾아서 반환한다.
- @RestController
  Return String - json 문자열로 직렬화되어 Return
  Return ModelAndView - viewName을 가지는 html 파일이 있는 경우 찾아서 반환
>

### 2. Forward

- `String`
    - 접두사 “forward:”와 함께 반환 되면 `UrlBasedViewResolver` 는 이를 리다이렉션이 발생해야 한다는 특별한 표시로 인식한다.
- `ModelAndView`
- `InternalResourceView`

```java
@Controller
public class ForwardController {

    @GetMapping("/hello")
    public String hello() {
        return "index";
    }

    @GetMapping("/forward1")
    public String forward1() {
        return "forward:/hello";
    }

    @GetMapping("/forward2")
    public ModelAndView forward2() {
        return new ModelAndView("forward:/hello");
    }

    @GetMapping("/forward3")
    public InternalResourceView forward3(HttpServletResponse httpServletResponse) throws IOException {
        return new InternalResourceView("/hello");
    }

}
```

### 3. Forward가 일어나는 위치는 어디일까?

**DispatcherServlet**

![https://blog.kakaocdn.net/dn/Sz6DV/btq9zjRpUGv/68Fw4fZtDwaNCZiCFx57oK/img.png](https://blog.kakaocdn.net/dn/Sz6DV/btq9zjRpUGv/68Fw4fZtDwaNCZiCFx57oK/img.png)

- 포워드의 경우 각각의 URL에 대해서 각각 인터셉터 통과하는 반면, 필터는 처음 요청했던 URL만 통과한다.

### 4. String, ModelAndView로 반환하는 경우 Spring은 redirect, forward를 어떻게 인식할까?

- String은 HandlerAdapter에서  ModelAndView로 감싸진다.

```java
public class UrlBasedViewResolver extends AbstractCachingViewResolver implements Ordered {
    public static final String REDIRECT_URL_PREFIX = "redirect:";
    public static final String FORWARD_URL_PREFIX = "forward:";

		protected View createView(String viewName, Locale locale) throws Exception {
        if (!this.canHandle(viewName, locale)) {
            return null;
        } else {
            String forwardUrl;
            if (viewName.startsWith("redirect:")) {
                forwardUrl = viewName.substring("redirect:".length());
                RedirectView view = new RedirectView(forwardUrl, this.isRedirectContextRelative(), this.isRedirectHttp10Compatible());
                String[] hosts = this.getRedirectHosts();
                if (hosts != null) {
                    view.setHosts(hosts);
                }

                return this.applyLifecycleMethods("redirect:", view);
            } else if (viewName.startsWith("forward:")) {
                forwardUrl = viewName.substring("forward:".length());
                InternalResourceView view = new InternalResourceView(forwardUrl);
                return this.applyLifecycleMethods("forward:", view);
            } else {
                return super.createView(viewName, locale);
            }
        }
    }
}
```

- `redirect:` 으로 시작하는 경우 `RedirectView` 으로 만들어서 리턴
- `forward:` 으로 시작하는 경우 `InternalResourceView` 으로 만들어서 리턴

![https://blog.kakaocdn.net/dn/wwwmy/btrfqEhmPHM/NoO6akCJOMkVZQPyIU1zQk/img.png](https://blog.kakaocdn.net/dn/wwwmy/btrfqEhmPHM/NoO6akCJOMkVZQPyIU1zQk/img.png)

- 즉, ModelAndView 객체는 ViewResovler(`UrlBasedViewResolver`)를 통해 View(`RedirectView` , `InternalResourceView`)로 반환된다.

## Reference

[https://developer.mozilla.org/ko/docs/Web/HTTP/Redirections](https://developer.mozilla.org/ko/docs/Web/HTTP/Redirections)

[https://www.baeldung.com/spring-redirect-and-forward](https://www.baeldung.com/spring-redirect-and-forward)