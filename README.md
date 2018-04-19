# Simple Spring app for aws demo

## Part 1 - The Controller

Requirements:
* JDK 1.8
* Maven 3
* Spring Boot 2.0.1

### Step 1
Create a new Maven project

### Step 2
Create the following folder structure. Create empty classes for now. We will address them soon
![](http://path/to/image);

### Step 3
Update the pom.xml file to the following

```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>io.froilan.aws</groupId>
    <artifactId>spring-boot</artifactId>
    <version>1.0-SNAPSHOT</version>

    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.0.1.RELEASE</version>
    </parent>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
    </dependencies>
    <properties>
        <java.version>1.8</java.version>
    </properties>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>
</project>
```

### Step 4
Add the following code to Application.java
```
package io.froilan.com.app;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.annotation.ComponentScan;

@SpringBootApplication
@ComponentScan(basePackages = "io.froilan.aws")
public class Application {

    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}
```

\* make sure to change the base package value to your package structure

### Step 5
Update the HelloController.java

```
package io.froilan.aws.controller;

import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class HelloController {

    @RequestMapping(value = "/", method = RequestMethod.GET)
    public String index() {
        return "Hello World!";
    }
}
```
\* Remember to change your package path accordingly

### Run it

in terminal, navagate to your project directory and use the following command:
```
mvn package && java -jar target/spring-boot-1.0-SNAPSHOT.jar
```

### Test it

Use Postman to test REST api endpoint
'http://localhost:8080/'
![](http://path/to/image)
