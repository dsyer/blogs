Spring Boot makes it easy to create Spring-powered, production-grade applications and services with absolute minimum fuss. It takes an opinionated view of the Spring platform so that new and existing users can quickly get to the bits they need. This article introduces Spring Boot and announces that the Spring Boot 0.5.0.M1 release is available.

You can use Spring Boot to create stand-alone Java applications that can be started using `java -jar` or more traditional WAR deployments. We also provide a command line tool that runs spring scripts.  The diagram below shows Spring Boot as a point of focus on the larger world of Spring projects of all kinds.  It presents a small surface area for users to approach and extract value from the rest of Spring.

![Spring Boot in Context](http://blog.springsource.org/wp-content/uploads/2013/08/boot-pics.png)

Our primary goals are:

* Provide a radically faster and widely accessible getting started experience for all Spring development
* Be opinionated out of the box, but get out of the way quickly as requirements start to diverge from the defaults
* Provide a range of non-functional features that are common to large classes of projects (e.g. embedded servers, security, metrics, health checks, externalized configuration)
* Absolutely no code generation and no requirement for XML configuration

<br/>

## Quick Start Script Example

The Spring Boot command line tool uses [Groovy](http://groovy.codehaus.org/) underneath so that we can write simple Spring snippets that can 'just run', for example:

```groovy
@Controller
class ThisWillActuallyRun {
	
	@RequestMapping("/")
	@ResponseBody
	String home() {
		return "Hello World!"
	}
	
}
```

Then in a shell:

```
$ spring run app.groovy
$ curl localhost:8080
Hello World!
```

<br/>

## Quick Start Java Example

If you don't want to use the command line tool, or you would rather work using Java and an IDE you can. Just add a `main()` method that calls `SpringApplication` and use `@EnableAutoConfiguration`:

```java
import org.springframework.boot.*;
import org.springframework.boot.autoconfiguration.*;
import org.springframework.stereotype.*;
import org.springframework.web.bind.annotation.*;

@Controller
@EnableAutoConfiguration
public class SampleController {

	@RequestMapping("/")
	@ResponseBody
	String home() {
		return "Hello World!"
	}

	public static void main(String[] args) throws Exception {
		SpringApplication.run(SampleController.class, args);
	}

}
```

_NOTE:_ the above example assumes your build system has a single dependency on the `spring-starter-web` project. Using Maven your configuration would look like this:

`pom.xml`

```xml
<pom>
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>0.5.0.M1</version>
    </parent>
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
            <version>${spring.boot.version}</version>
        </dependency>
    </dependencies>
    <build>
        <plugins>
            <plugin>
                <groupId>${project.groupId}</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>
</pom>
```

<br/>

## Downloading the CLI

Spring Boot CLI can be downloaded from our Maven repository, and then you can use a shell `alias` for the Spring Boot command line tool:

    $ wget http://maven.springframework.org/milestone/org/springframework/boot/spring-boot-cli/0.5.0.M1/spring-boot-cli-0.5.0.M1.jar
    $ alias spring="java -jar `pwd`/spring-boot-cli-0.5.0.M1.jar"

<br/>

## Spring Boot Modules
There are a number of modules in Spring Boot. Here are the important ones:

### Spring Boot

The main library providing features that support the other parts of Spring Boot, these include:

* The `SpringApplication` class, providing static convenience methods that make it easy to write a stand-alone Spring Application. Its sole job is to create and refresh an appropriate Spring `ApplicationContext`
* Embedded web applications with a choice of container (Tomcat or Jetty for now)
* First class externalized configuration support
* Convenience `ApplicationContext` initializers, including support for sensible logging defaults.

_See [spring-boot/README.md](https://github.com/SpringSource/spring-boot/tree/master/README.md)._


### Spring Boot - Autoconfigure

Spring Boot can configure large parts of common applications based on the content of their classpath. A single `@EnableAutoConfiguration` annotation triggers auto-configuration of the Spring context.

Auto-configuration attempts to deduce which beans a user might need. For example, If 'HSQLDB' is on the classpath, and the user has not configured any database connections, then they probably want an in-memory database to be defined. Auto-configuration will always back away as the user starts to define their own beans.

_See [spring-boot-autoconfigure/README.md](https://github.com/SpringSource/spring-boot/tree/master/spring-boot-autoconfigure/README.md)._


<br/>

### Spring Boot - Starters

Starters are a set of convenient dependency descriptors that you can include in your application. You get a one-stop-shop for all the Spring and related technology that you need without having to hunt through sample code and copy paste loads of dependency descriptors. For example, if you want to get started using Spring and JPA for database access just include the `spring-boot-starter-data-jpa` dependency in your project, and you are good to go.

_See [spring-boot-starters/README.md](https://github.com/SpringSource/spring-boot/tree/master/spring-boot-starters/README.md)._


<br/>

### Spring Boot - CLI

The Spring command line application compiles and runs Groovy source, making it super easy to write the absolute minimum of code to get an application running. Spring CLI can also watch files, automatically recompiling and restarting when they change.

<br/>

### Spring Boot - Actuator

Spring Boot Actuator provides additional auto-configuration to decorate your application with features that make it instantly deployable and supportable in production.  For instance if you are writing a JSON web service then it will provide a server, security, logging, externalized configuration, management endpoints, an audit abstraction, and more. If you want to switch off the built in features, or extend or replace them, it makes that really easy as well.

<br/>

### Spring Boot - Tools 

Spring Boot Tools provides helpers for building and running executable JAR and WAR archives. The Loader provides the secret sauce that allows you to build a single jar file that can be launched using `java -jar`. Generally you will not need to use `spring-boot-loader` directly but instead work with the [Gradle](https://github.com/SpringSource/spring-boot/tree/master/spring-boot-tools/spring-boot-gradle-plugin/README.md) or [Maven](https://github.com/SpringSource/spring-boot/tree/master/spring-boot-tools/spring-boot-gradle-plugin/README.md) plugin.


<br/>

## Samples

Groovy samples for use with the command line application are available in [spring-boot-cli/samples](https://github.com/SpringSource/spring-boot/tree/master/spring-boot-cli/samples/#). To run the CLI samples type `spring run <sample>.groovy` from samples directory.

Java samples are available in [spring-boot-samples](https://github.com/SpringSource/spring-boot/tree/master/spring-boot-samples/#) and most (except the ones that build to WAR files) can be build with maven and run use `java -jar target/<sample>.jar`.

<br/>

## Contributing to Spring Boot

Spring Boot is released under the non-restrictive Apache 2.0 license. If you would like to contribute something, or simply want to hack on the code this document should help you get started. You can raise issues in Github as well, and there is also a [Pivotal Tracker Project](https://www.pivotaltracker.com/s/projects/804393) that we use to manage priorities and collect feature requests.


## SpringOne 2GX 2013 is around the corner

Book your place at [SpringOne in Santa Clara](http://www.springone2gx.com/conference/santa_clara/2013/09/home) soon. It's simply the best opportunity to find out first hand all that's going on and to provide direct feedback. Expect a number of significant new announcements this year. Check recent blog posts to see what I mean and there is more to come!
