# Web application
## 0. Servlet
- 주로 웹서버에서 동작하는 애플리케이션을 구현할 때 사용
- 과거에는 CGI(Common Gateway Interface)라는 웹서버에서 외부 프로그램을 호출하면 수행 결과를 브라우저로 전송하는 기술을 사용
    - 동시 접속자가 많아질수록 처리 효율 저하(응답 시간 증가)
- 이에 따라 등장하게 된 서블릿은 클라이언트에 서비스하기 위한 목적으로 만들어짐
## 1. 웹 애플리케이션 접근
`http://12.34.567.89:8080/edu/index.jsp` 라는 URL이 있다고 가정하자.
- `12.34.567.89`: 웹 애플리케이션을 서비스하는 웹서버가 설치된 컴퓨터 주소. IP 주소나 도메인 이름으로 지정한다.
- `:8080`: 서버를 찾아가기 위한 포트 번호. 컴퓨터 주소로 찾은 컴퓨터에서 웹서버를 찾는데 사용한다.
- `/edu`: 애플리케이션명. 웹서버는 애플리케이션 단위로 서비스한다.
- `/index.jsp`: 클라이언트가 요청한 최종 문서정보. 파일시스템의 디렉터리 구조 형태로 지정된다.
## 2. 웹 애플리케이션 구조
![Web application structure](https://user-images.githubusercontent.com/51183274/135567950-3fb0aa90-c45a-41bf-b774-0d70d53d60a0.png)
- 웹 애플리케이션은 하나의 디렉터리로 이 안에 서비스하려는 문서들을 만들어 놓으면 웹에서 서비스 됨.
- 반드시 모든 웹 애플리케이션이 공통적으로 가져야 하는 디렉터리와 파일
    - `/WEB-INF/web.xml`: 환경설정
    - `/WEB-INF/classes/*`: 웹 애플리케이션에서 서비스하려는 클래스 파일
    - `/WEB-INF/lib/*`: jar로 압축된 클래스 파일들
- web.xml과 클래스 파일을 제외한 다른 파일들은 어디에 있으나 무관
## 3. 환경설정 파일: web.xml
- web.xml은 웹 애플리케이션의 서비스 처리에 관련된 내용이 정의된 파일
- 웹서버가 서비스를 구동할 때 web.xml 파일을 읽어 클라이언트에 서비스하기 위한 웹 애플리케이션을 준비하는 작업을 수행
- 직접 만들지 않은 웹 애플리케이션을 유지보수해야 한다면 제일먼저 web.xml을 비롯한 환경설정 파일들을 분석해야 함.
    - 순수 웹 애플리케이션으로 개발된 경우 web.xml 분석만으로도 전체 흐름 파악 용이
- 잘못 정의한 web.xml은 서비스에 필요한 준비 설정을 올바르게 할 수 없어 애플리케이션 내 모든 파일들이 서비스되지 않음.
# 서블릿 구현
## 1. 서블릿 클래스 간의 관계
- 서블릿을 구현할 때 반드시 상속받아야 하는 클래스: **HttpServlet**
    - 웹상에서 클라이언트 요청이 있을 때 해당 서블릿을 실행하는 모든 조건이 포함되어 있음
- 서블릿 프로그램의 클래스 계층 구조

  ![image](https://user-images.githubusercontent.com/51183274/135569555-abbfdb53-9111-4fdd-9d8f-f7b0b5499d00.png)
### Servlet 인터페이스
- 서블릿 프로그램을 개발할 때 반드시 구현해야 하는 메소드를 선언하고 있는 인터페이스
- 서블릿 프로그램의 실행주기와 연간된 메소드들을 선언
    - `init()`
    - `service()`
    - `destroy()`
    - `getServletConfig()`
    - `getServletInfo()`
### GenericServlet 클래스
- Servlet 인터페이스를 상속하여 클라이언트-서버 환경에서 서버단의 애플리케이션으로써 필요한 기능을 구현한 추상 클래스
- `service()` 메소드를 제외한 모든 메소드를 재정의하여 적절한 기능으로 구현
- 이 클래스를 상속하면 애플리케이션의 프로토콜에 따라 메소드 재정의 구문을 적용해야 함.
### HttpServlet 클래스
- GenericServlet 클래스를 상속하여 `service()` 메소드를 재정의함으로써 HTTP 프로토콜에 알맞은 동작을 수행하도록 구현한 클래스
- HTTP 프로토콜 기반으로 브라우저로부터 요청을 전달받아 처리하는 클래스
- `service()` 메소드에는 요청방식에 따라 정해진 사양의 메소드가 호출되도록 구현되어 있음.
- 반드시 재정의해야 하는 메소드는 없고 요청 방식에 따라 필요한 메소드를 재정의하여 구현