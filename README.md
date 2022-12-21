# Adding a Servlet Class

## Learning Goals

- Configure `pom.xml` for the Servlet API.
- Create a servlet to handle a GET request.

## Code Along

## Introduction

We will add a servlet to the web application project from the previous lesson.
The servlet will generate a static web page as a response to a GET request.

## Configure `pom.xml` for Servlet API

We will create a Java servlet named `GreetServlet` to generate a dynamic greeting based on data
sent from the client.  Recall that a servlet class should extend `HttpServlet`.
We need to update `pom.xml` to add a dependency for the servlet api:

```xml
    <!-- https://mvnrepository.com/artifact/jakarta.servlet/jakarta.servlet-api -->
    <dependency>
      <groupId>javax.servlet</groupId>
      <artifactId>javax.servlet-api</artifactId>
      <version>4.0.1</version>
      <scope>provided</scope>
    </dependency>
```

Nest the dependency within the `dependencies` element:

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <groupId>org.example</groupId>
  <artifactId>servlets</artifactId>
  <packaging>war</packaging>
  <version>1.0-SNAPSHOT</version>

  <!-- Set compiler to Java11 -->
  <properties>
    <maven.compiler.source>11</maven.compiler.source>
    <maven.compiler.target>11</maven.compiler.target>
  </properties>

  <name>servlets Maven Webapp</name>
  <url>http://maven.apache.org</url>
  <dependencies>

    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>3.8.1</version>
      <scope>test</scope>
    </dependency>

    <!-- https://mvnrepository.com/artifact/jakarta.servlet/jakarta.servlet-api -->
    <dependency>
      <groupId>javax.servlet</groupId>
      <artifactId>javax.servlet-api</artifactId>
      <version>4.0.1</version>
      <scope>provided</scope>
    </dependency>

  </dependencies>

  <build>
    <finalName>servlets</finalName>

    <!-- Tomcat9 Maven Plugin -->
    <plugins>
      <plugin>
        <groupId>org.opoo.maven</groupId>
        <artifactId>tomcat9-maven-plugin</artifactId>
        <version>3.0.1</version>
        <configuration>
          <path>/</path>
        </configuration>
      </plugin>
    </plugins>

  </build>
</project>
```

Don't forget to reload Maven after making changes to `pom.xml`:

![reload maven icon](https://curriculum-content.s3.amazonaws.com/6036/create-webapp-project/reload_maven.png)


## Create a new servlet class named `GreetServlet`

Now we can create a new Java class named `GreetServlet`.  Right-click on the `java` directory
and create the new class named `GreetServlet`.  Your project should look like this:

![New Servlet Class](https://curriculum-content.s3.amazonaws.com/6036/java-mod-5-servlet-class/greetservlet_class.png)

Copy the following code into `GreetServlet`:

```java
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.io.PrintWriter;

@WebServlet(urlPatterns = "/greet")
public class GreetServlet extends HttpServlet {

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp)
            throws ServletException, IOException {
        // Prepare the server response
        resp.setContentType("text/html; charset=UTF-8");
        PrintWriter out = resp.getWriter();
        out.println("<html>");
        out.println("<head><title>Servlet Example</title></head>");
        out.println("<body>");
        out.println("<h1>Greetings!</h1>");
        out.println("</body>");
        out.println("</html>");
    }

}
```

Let's explore what the code does:

1. The code `@WebServlet(urlPatterns = "/greet")`  tells the servlet container that the servlet
   will handle requests for the path **http://localhost:8080/greet**.
2. The code `public class GreetServlet extends HttpServlet ` makes our class a servlet that
   can override inherited methods from `HttpServlet` to handle HTTP requests.
3. We override the `doGet()` method to handle incoming **HTTP GET** requests to the URL **http://localhost:8080/greet**.
   - The method takes two parameters:
     - `HttpServletRequest req` stores information about the incoming HTTP GET request from the client.
     - `HttpServletResponse resp` will be used to generate an HTTP response that will be sent to the client.
        We can see the `doGet()` method create a simple HTML web page as a response.

## Restart Tomcat

While the embedded Tomcat server recognizes and reloads changes
to the JSP files in the webapp folder, we will need to stop the server
and restart it for changes made to the servlet class.

1. Stop executing the web application by pressing the red stop icon on the Maven toolbar:    
   ![stop Tomcat server](https://curriculum-content.s3.amazonaws.com/6036/java-mod-5-servlet-class/stop_server.png)
2. Double-click on **tomcat9:run** to restart the web application:       
   ![tomcat plugin](https://curriculum-content.s3.amazonaws.com/6036/create-webapp-project/run_tomcat.png)

Now we should be able to run the servlet using the URL set in `@WebServlet` annotation.
Enter **http://localhost:8080/greet** in the browser:

![greet servlet in browser](https://curriculum-content.s3.amazonaws.com/6036/java-mod-5-servlet-class/greetservlet_path.png)


## Conclusion

A Java servlet class that extends `HttpServlet` can override inherited methods
associated with HTTP verbs such as `doGet()`.  The method takes two parameters
that represent the HTTP request and response.

## Resources

- [HttpServlet](https://docs.oracle.com/javaee/7/api/javax/servlet/http/HttpServlet.html)   
- [HttpServletRequest](https://docs.oracle.com/javaee/7/api/javax/servlet/http/HttpServletRequest.html)       
- [HttpServletResponse](https://docs.oracle.com/javaee/7/api/javax/servlet/http/HttpServletResponse.html)   

