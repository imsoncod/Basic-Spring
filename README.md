# Basic-Spring
🍃 Spring 🍃

<br>

## 목표

* 스프링 MVC 프로젝트를 만들어 실행할 수 있다

* 스프링 기본 코드들을 이해할 수 있다

* 스프링 MVC 구조의 동작 과정을 이해할 수 있다

<br>

## 프로젝트 구성

  ![image](https://user-images.githubusercontent.com/48934537/79068783-6bca9080-7d04-11ea-8186-f0bf83c18c4d.png)
  
  * src/main/java : java파일이 모여있는 디렉토리
  
  * src/main/resources : 후에 스프링 설정 파일이나 쿼리가 저장될 디렉토리
  
  * src/test : TDD(Test Driven Development) 방법론이나 테스트코드를 따로 작성하는 디렉토리
  
  * src/main/webapp : 메이븐의 기본폴더, 모든 jsp, js등의 파일이 저장됨
  
  * servlet-context.xml / root-context.xml : 서블릿 관련 설정 파일
  
  * web.xml : 배포서술자(DD)라고 불리며 환경설정을 담당(세션유지시간, 서블릿 매핑, Welcome File list 등을 정의)
  
  * pom.xml : 라이브러리 관리 파일, <dependency>태그 하나가 하나의 라이브러리를 의미
 
<br>

## 동작 과정(그림)

  ![image](https://user-images.githubusercontent.com/48934537/79069258-c74a4d80-7d07-11ea-8d44-758c18354d65.png)

  * 실행순서
  
    Request -> DispatcherServlet -> HandlerMapping -> Controller -> DB
  
    -> Controller -> DispatcherServlet -> ViewResolver -> View -> Response
    
<br>
  
## 동작 과정(설명)

  1. 클라이언트가 Request 요청을 보내면 DispatcherServlet이 요청을 가로챈다
  
     web.xml - url-pattern 태그에 등록된 내용만을 가로챔     
     
```xml
<servlet>
  <servlet-name>appServlet</servlet-name>
  <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    <init-param>
      <param-name>contextConfigLocation</param-name>
      <param-value>/WEB-INF/spring/appServlet/servlet-context.xml</param-value>
    </init-param>
    <load-on-startup>1</load-on-startup>
</servlet>
		
<servlet-mapping>
  <servlet-name>appServlet</servlet-name>
  <url-pattern>.do</url-pattern> <!-- .do만 가로챔 -->
</servlet-mapping>
```

  2. DispatcherServlet이 가로챈 요청을 HandlerMapping에게 보내 해당 요청을 처리할 수 있는 Controller을 찾는다
  
  3. Controller 로직 수행
  
  4. Servlet_context.xml의 ViewResolver를 통해 view를 찾는다
  
```xml
<beans:bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
  <beans:property name="prefix" value="/WEB-INF/views/" /> <!--Controller에서 리턴된 View이름 앞에 붙음 -->
  <beans:property name="suffix" value=".jsp" /> <!--Controller에서 리턴된 View이름 뒤에 붙음 -->
</beans:bean>
```

  5. view를 최종 클라이언트에게 전송한다
  
<br>

## Controller 코드 분석

```java
@Controller
public class HomeController {
	
	private static final Logger logger = LoggerFactory.getLogger(HomeController.class);
	
	/**
	 * Simply selects the home view to render by returning its name.
	 */
	@RequestMapping(value = "/", method = RequestMethod.GET)
	public String home(Locale locale, Model model) {
		logger.info("Welcome home! The client locale is {}.", locale);
		
		Date date = new Date();
		DateFormat dateFormat = DateFormat.getDateTimeInstance(DateFormat.LONG, DateFormat.LONG, locale);
		
		String formattedDate = dateFormat.format(date);
		
		model.addAttribute("serverTime", formattedDate );
		
		return "home";
	}
	
}
```

  1. @RequestMapping : value에 해당하는 url로 요청이 오면 해당 메소드를 실행시킨다
  
  2. model.addAttribute(key, value) : model을 이용하여 view에 데이터를 전달할 수 있다
  
  3. return "home" : ViewResolver에게 전달되고 prefix, suffix와 결합되어 클라이언트에게 전송된다
  
<br>

## 마무리

구조를 변형하여 프로젝트를 수행해볼 예정, 좀 더 이해하고 공부해야 할 것 같다
