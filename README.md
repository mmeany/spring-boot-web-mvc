
# Overview
The purpose of this project is to document what is required to get a Spring Boot application started that makes use of Web MVC and the Apache Tiles 2 framework.

# Changes

* Start by creating an application based on spring-boot-starter-web (1.1.4.RELEASE)
* Add dependencies to the POM to pull in Tomcat and Jasper
```
	<dependency>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-tomcat</artifactId>
		<scope>provided</scope>
	</dependency>
	<dependency>
		<groupId>org.apache.tomcat.embed</groupId>
		<artifactId>tomcat-embed-jasper</artifactId>
		<scope>provided</scope>
	</dependency>
```
* Change the packaging to WAR in the POM
```
	<packaging>war</packaging>
```
* Add properties to redefine the application base in the POM
```
	<main.basedir>${basedir}/../..</main.basedir>
	<m2eclipse.wtp.contextRoot>/</m2eclipse.wtp.contextRoot>
```
* Add view resolver properties to application.properties
```
	spring.view.prefix: /WEB-INF/tiles/view/
	spring.view.suffix: .jsp
```
* Create Web application structure the Maven way:
```
	src/main/webapps
		static
			index.html
		WEB-INF
			tiles
				view
					greeting.jsp
```
    Put any old rubbish in JSP and HTML files for now, we just want to test the routing
* Add a controller class (GreetingController.java) into a directory under that containing Application.java
```
        package com.mvmlabs.springboot.web;
        
        import org.apache.commons.logging.Log;
        import org.apache.commons.logging.LogFactory;
        import org.springframework.stereotype.Controller;
        import org.springframework.ui.Model;
        import org.springframework.web.bind.annotation.PathVariable;
        import org.springframework.web.bind.annotation.RequestMapping;
        import org.springframework.web.bind.annotation.RequestMethod;
        import org.springframework.web.bind.annotation.RequestParam;
        import org.springframework.web.servlet.ModelAndView;
        
        @Controller
        public class GreetingController {
        	private Log log = LogFactory.getLog(this.getClass());
        
        	@RequestMapping(value = "/greet", method=RequestMethod.GET)
        	public ModelAndView greet(@RequestParam(value = "name", required=false, defaultValue="World!")final String name, final Model model) {
        		log.info("Controller has been invoked with Request Parameter name = '" + name + "'.");
        		return new ModelAndView("greetings", "name", name);
        	}
        
        	@RequestMapping(value = "/greet/{name}", method=RequestMethod.GET)
        	public ModelAndView greetTwoWays(@PathVariable(value="name") final String name, final Model model) {
        		log.info("Controller has been invoked with Path Variable name = '" + name + "'.");
        		return new ModelAndView("greetings", "name", name);
        	}
        }
```
* Build the project using Maven
```
	mvn clean package
```
* Run it, the --debug flag is to display Springs Auto-Configuration Report
```
	java -jar target\spring-boot-web-mvc-1.0.war --debug
```
* Confirm static content can be accessed:
```
	http://localhost:8080/static/index.html
```
* Confirm that Spring MVC has been configured as expected
```
	http://localhost:8080/greet?name=Mark
	http://localhost:8080/greet/Mark
```

All done here.

Note: There is a structue that implies use of Apache Tiles, that is where this example is heading. First though I wanted a plain and simple starting point
