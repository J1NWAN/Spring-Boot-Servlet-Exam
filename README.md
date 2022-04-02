# 김영한 선생님의 Spring MVC 1편 정리

## 서블릿
``` java
@WebServlet(name = "helloServlet", urlPatterns = "/hello")
public class HelloServlet extends HttpServlet {

    @Override
    protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        // 애플리케이션 로직
    }
}
```
- 서블릿은 단순하다. 위의 코드처럼 구성되었으며 urlPatterns에 맞게 호출이 된다.
- HTTP **요청**이면 HttpServletRequest로 사용한다.
- HTTP **응답**이면 HttpServletResponse로 제공한다.
- HTTP 스펙을 편리하게 사용할 수 있다. (기본적인 HTTP 스펙을 알고 있어야한다.)
