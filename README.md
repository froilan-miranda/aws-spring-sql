# Simple Spring app for aws demo
\* props to MyDevGeek, this is just an update to one of their tutorials
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
        <version>1.5.10.RELEASE</version>
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


## Part 2 - The Model

Requirements:
* JDK 1.8
* Maven 3
* Spring Boot 2.0.1
* MySQL database


### Step 1
Create a MySql database to store our tables;

### Step 2
Create a 'user' table inside our new database
```
DROP TABLE IF EXISTS `user`;

CREATE TABLE `user` (
`id` int(11) unsigned NOT NULL AUTO_INCREMENT,
`first_name` varchar(11) DEFAULT NULL,
`last_name` varchar(11) DEFAULT NULL,
`username` varchar(11) DEFAULT NULL,
`created` datetime DEFAULT NULL,
`updated` datetime DEFAULT NULL,
PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=2 DEFAULT CHARSET=latin1;

LOCK TABLES `user` WRITE;

INSERT INTO `user` (`id`, `first_name`, `last_name`, `username`, `created`, `updated`)
VALUES
(1,'Bruce','Banner','test','2017-02-22 00:00:00','2017-02-22 00:00:00');
```

### Step 3
Add the mysql and jpa dependencies to our pom.xml file
```
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>6.0.6</version>
</dependency>
```
```
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-jpa</artifactId>
    <version>1.5.12.RELEASE</version>
</dependency>
```

### Step 4
Add some more packages for later and a 'application.properties' file inside the resources folder

![](http://path/to/image)
```
spring.datasource.url=jdbc:mysql://localhost:3306/spring_aws?useUnicode=true&useJDBCCompliantTimezoneShift=true&useLegacyDatetimeCode=false&serverTimezone=UTC
spring.datasource.username=root
spring.datasource.password=
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
spring.jpa.database-platform=org.hibernate.dialect.MySQL5Dialect
spring.jpa.hibernate.ddl-auto=update
```
### Step 5
We need to configure spring to look in the right packages for our repo and entity. Here is the new code for Application.java
```
package io.froilan.aws.app;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.boot.autoconfigure.domain.EntityScan;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.data.jpa.repository.config.EnableJpaRepositories;

@SpringBootApplication
@ComponentScan(basePackages = "io.froilan.aws")
@EnableJpaRepositories(basePackages = "io.froilan.aws.repo")
@EntityScan(basePackages = "io.froilan.aws.domain")
public class Application {

    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}
```

### Step 6
Create User.java in our domain package
```
package io.froilan.aws.domain;

import org.hibernate.annotations.CreationTimestamp;
import org.hibernate.annotations.UpdateTimestamp;

import javax.persistence.*;
import java.io.Serializable;
import java.sql.Date;

@Entity
@Table(name = "user")
public class User implements Serializable {

    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    @Column(name = "id")
    private Long id;

    @Column(name = "first_name")
    private String firstName;

    @Column(name = "last_name")
    private String lastName;

    @Column(name = "username")
    private String username;

    @Column(name = "created")
    @CreationTimestamp
    private Date created;

    @Column(name = "updated")
    @UpdateTimestamp
    private Date updated;

    //getters and setters

}
```
### Step 7
Now lets create UserRepository.java in the repo package
```
package io.froilan.aws.repo;

import io.froilan.aws.domain.User;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;

@Repository
public interface UserRepository extends JpaRepository<User, Long> {
}
```

### Step 8
Create UserController.java inside the controller package
```
package io.froilan.aws.controller;

import io.froilan.aws.domain.User;
import io.froilan.aws.repo.UserRepository;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.http.HttpStatus;
import org.springframework.web.bind.annotation.*;

@RestController
@RequestMapping("/user")
public class UserController {

    @Autowired
    private UserRepository userRepository;

    @RequestMapping(value = "/{id}", method = RequestMethod.GET)
    @ResponseStatus(HttpStatus.OK)
    public User getUser(@PathVariable("id") Long id) {
        return userRepository.findOne(id);
    }
}
```
### Run it
go to the project directory. Run 'mvn spring-boot:run'

### Test it
Use Postman again to test your end point

## Part 3 - Deployment
