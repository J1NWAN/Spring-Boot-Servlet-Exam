# 김영한 선생님의 Spring MVC 1편

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
<br>

## 서블릿 흐름
![img1 daumcdn](https://user-images.githubusercontent.com/61576254/161426647-bfe0cfed-c809-4b40-9e25-05cb3fdb0b33.png)
### HTTP 요청, 응답 흐름
1. WAS는 Request, Response 객체를 생성해서 서블릿 객체를 호출
2. 개발자는 Request 객체에서 HTTP 요청 정보를 꺼내서 사용
3. 개발자는 Response 객체에 HTTP 응답 정보를 입력
4. WAS는 Response 객체의 내용으로 HTTP 응답 정보를 생성
<br>

## 서블릿 컨테이너
- 톰캣처럼 서블릿을 지원하는 WAS를 서블릿 컨테이너라고 함
- 서블릿 컨테이너는 서블릿 객체를 생성, 초기화, 호출, 종료하는 생명주기를 관리
- 서블릿 객체는 싱글톤으로 관리
    - 요청마다 생성은 낭비
    - 최초 로딩 시점에 객체를 생성해 놓고 재활용
    - 공유 변수 사용 주의
    - 서블릿 컨테이너 종료 시 함께 종료
- JSP도 서블릿으로 변환 되어서 사용
- 동시에 요청을 위한 멀티 쓰레드 처리 지원

### WAS의 멀티 쓰레드 지원(핵심)
- WAS가 멀티 쓰레드 부분을 처리한다.
- 개발자가 멀티 쓰레드 관련 코드를 신경쓰지 않아도 된다.
- 개발자는 마치 싱글 쓰레드처럼 편리하게 개발한다.
- 단, 싱글톤 객체를 주의해서 사용해야 한다.
<br>

## 스프링 부트 서블릿 환경 구성
**@ServletComponentScan**를 main메소드가 있는 클래스에 붙이면 된다.
``` java
@ServletComponentScan // 서블릿 자동 등록
@SpringBootApplication
public class ServletApplication {
    public static void main(String[] args) {...}
}
```
<br>

**@WebServlet**를 이용하여 서블릿 코드를 만든다.
- name: 서블릿 이름
- urlPatterns: URL 매핑
``` java
@WebServlet(name = "helloServlet", urlPatterns = "/hello")
public class HelloServlet extends HttpServlet {

    @Override
    protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        String username = req.getParameter("username");
        System.out.println("username = " + username);

        resp.setContentType("text/plain");
        resp.setCharacterEncoding("utf-8");
        resp.getWriter().write("hello " + username);
    }
}
```
**주의: name, urlPatterns이 중복되면 컴파일 에러가 발생한다.**
<br>

## 서블릿 컨테이너 동작 방식
### 내장 톰캣 서버 생성
![img1 daumcdn](https://user-images.githubusercontent.com/61576254/161427350-61953b1b-0cc1-490d-9d81-a715d9d22f9b.png)
<br>

### 웹 애플리케이션 서버의 요청 응답 구조
![img1 daumcdn](https://user-images.githubusercontent.com/61576254/161427405-ab7affc7-e3d6-49d7-a929-c9f7ee618c4d.png)
<br>

## HttpServletRequest
**역할**
- 서블릿은 HTTP 요청 메세지를 파싱하고 그 결과를 HttpServletRequest 객체로 제공.

### HttpServletRequest의 부가적인 기능
1. 임시 저장소 기능: 사용자의 요청이 들어오고 끝날 때까지의 생명주기를 이용하여 임시 저장소 기능 역할도 갖는다.
    - 저장: request.setAttribute(name, value)
    - 조회: request.getAttribute(name)
2. 세션 관리 기능
    - request.getSession(create: true) 
<br>

## HTTP 요청 데이터
**서버로 데이터를 전달하는 방법은 3가지가 있다.**

1. GET 방식
    - URL의 쿼리 파라미터 데이터를 포함해서 전달
2. POST 방식
    - HTML form 태그 사용
3. HTTP message Body
    - HTTP API에서 주로 사용(JSON, XML TEXT)
