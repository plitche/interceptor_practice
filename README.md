# interceptor_practice

https://www.youtube.com/watch?v=6rNZFo4eyhE
https://www.youtube.com/watch?v=6tALfw_mSB8

---

### Interceptor
* 컨트롤러(Controller)의 '핸들러(Handler)'를 호출하기 전과 후에 요청과 응답을 참조하거나 가공할수 있는 일종의 필터  
  
사용자 요청에 의해 서버에 들어온 Request 객체를 컨트롤러의 핸들러(사용자가 요청한 url에 따라 실행되어야 할 메서드, 이하 핸들러)로  
도달하기전에 개발자가 원하는 추가적인 작업을 한후 핸들러로 보낼수 있도록 해주는것이 인터셉터

### Why
* 개발자는 특정 Controller의 핸들러가 실행되기 전이나 후에 추가적인 작업을 원할때 Interceptor를 사용한다.  
  
### How
스프링에서 제공하는 org.springframework.web.servlet.HandlerInterceptor 인터페이스를 구현하거나,  
org.springframework.web.servlet.handler.HandlerInterceptorAdapter 추상클래스를 오버라이딩 함으로써  
자신만의 인터셉터를 만들수 있다. HandlerInterceptorAdapter 추상클래스 경우  HandlerInterceptor 인터페이스를 상속받아 구현  
  
### Method
스프링이 제공해주는 HandlerInterceptor 인터페이스와 HandlerInterceptorAdapter 추상클래스에 정의되어 있는  
메서드는 preHandle(), postHandle(), afterCompletion() 3가지  
  
1. preHandle  
컨트롤러가 호출되기 전에 실행됨  
컨트롤러가 실행 이전에 처리해야 할 작업이 있는경우 혹은 요청정보를 가공하거나 추가하는경우 사용  
실행되어야 할 '핸들러'에 대한 정보를 인자값으로 받기때문에 '서블릿 필터'에 비해 세밀하게 로직을 구성할수 있음  
리턴값이 boolean이다. 리턴이 true 일경우 preHandle() 실행후 핸들러에 접근  
false일경우 작업을 중단하기 때문에 컨트롤러와 남은 인터셉터가 실행되지 않음  

2. postHandle  
핸들러가 실행은 완료 되었지만 아직 View가 생성되기 이전에 호출  
ModelAndView 타입의 정보가 인자값으로 받는다. 따라서 Controller에서 View 정보를 전달하기 위해 작업한 Model 객체의 정보를 참조하거나 조작할 수 있음  
preHandle() 에서 리턴값이 fasle인경우 실행되지않음  
적용중인 인터셉터가 여러개 인경우, preHandle()는 역순으로 호출됨  
비동기적 요청처리 시에는 처리되지않음  
  
3. afterCompletion
모든 View에서 최종 결과를 생성하는 일을 포함한 모든 작업이 완료된 후에 실행됨  
요청 처리중에 사용한 리소스를 반환해주기 적당한 메서드  
preHandle() 에서 리턴값이 false인경우 실행되지 않음  
적용중인 인터셉터가 여러개인경우 preHandle()는 역순으로 호출됨  
비동기적 요청 처리시에 호출되지않음  
  
### Process
1. 사용자는 서버에 자신이 원하는 작업을 요청하기 위해 url을 통해 Request 객체를 보냄  
2. DispatcherServlet은 해당 Request 객체를 받아서 분석한뒤 '핸들러 매핑(HandlerMapping)' 에게 사용자의 요청을 처리할 핸들러를 찾도록 요청  
3. 그 결과로 핸들러 실행체인(HandlerExectuonChanin)이 동작하게 되는데, 이 핸들러 실행체인은 하나 이상의 핸들러 인터셉터를 거쳐서 컨트롤러가 실행될수 있도록 구성  

> 핸들러 인터셉터를 등록하지 않았다면, 곧바로 컨트롤러가 실행된다. 반대로 하나이상의 인터셉터가 지정되어 있다면 지정된 순서에 따라서 인터셉터를 거쳐서 컨트롤러를 실행한다)

