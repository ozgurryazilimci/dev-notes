# Maven Project Setup with IntelliJ IDEA

This page explains how to create and open a Maven-based Spring Boot project in IntelliJ IDEA, how the standard project
structure looks, and includes an example `pom.xml`.

---

## What is Maven?

Apache Maven is a build and dependency management tool for Java projects.  
It uses a configuration file called `pom.xml` (Project Object Model) to define:

- Project metadata
- Dependencies
- Build configuration
- Java version
- Plugins

---

## Example `pom.xml`

Below is a sample **parent Maven project** using Spring Boot and Java 21.

<details>
<summary>pom.xml</summary>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>

  <parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>3.5.4</version>
    <relativePath/>
  </parent>

  <groupId>com.example</groupId>
  <artifactId>sample-maven-project</artifactId>
  <version>1.0.0-SNAPSHOT</version>
  <packaging>pom</packaging>

  <name>${project.groupId}:${project.artifactId}:${project.version}</name>
  <description>Sample Maven Project</description>

  <properties>
    <!-- Java version -->
    <java.version>21</java.version>

    <maven.compiler.source>21</maven.compiler.source>
    <maven.compiler.target>21</maven.compiler.target>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
  </properties>

  <dependencies>

    <!-- Spring Boot -->
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter</artifactId>
    </dependency>

    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-web</artifactId>
    </dependency>

    <!-- Test -->
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-test</artifactId>
      <scope>test</scope>
    </dependency>

  </dependencies>

</project>
```

</details>

---

## Adding Additional Dependencies

Gradle allows you to add dependencies using the `dependencies` block inside `pom.xml`.

---

### Swagger (OpenAPI) Support

To enable Swagger UI and OpenAPI documentation, add the following dependency:

```xml
<!-- Swagger Support -->
<dependency>
  <groupId>org.springdoc</groupId>
  <artifactId>springdoc-openapi-starter-webmvc-ui</artifactId>
  <version>2.5.0</version>
</dependency>
```

Swagger UI and OpenAPI endpoints will be available at  
`/swagger-ui/index.html` and `/v3/api-docs`.

---

### Logging Support (SLF4J)

To add SLF4J API for logging abstraction:

```xml
  <!-- Log Support -->
<dependency>
  <groupId>org.slf4j</groupId>
  <artifactId>slf4j-api</artifactId>
</dependency>
```

Spring Boot already provides a default logging implementation (Logback), so no additional configuration is required.

---

## Opening a Maven Project in IntelliJ IDEA

1. Open **IntelliJ IDEA**
2. Click **File → Open**
3. Select the folder that contains `pom.xml`
4. Click **OK**
5. IntelliJ will automatically detect the Maven project
6. When prompted, choose **Load Maven Project**

IntelliJ will download all dependencies and index the project.

---

## Standard Maven Project Structure

After importing, you will see the following structure:

    sample-maven-project
    ├── pom.xml
    └── src
        ├── main
        │   ├── java
        │   │   └── com.example
        │   │       └── Application.java
        │   └── resources
        │       └── application.yml
        └── test
            └── java
                └── com.example
                    └── ApplicationTests.java

---

## Source Roots in IntelliJ

IntelliJ automatically marks folders with correct roles:

- `src/main/java` → **Sources Root**
- `src/main/resources` → **Resources Root**
- `src/test/java` → **Test Sources Root**
- `src/test/resources` → **Test Resources Root**

You can verify or change this by:

- Right-click folder
- Mark Directory As → (Sources / Test Sources / Resources)

---

## Running the Application

1. Open the main class (annotated with `@SpringBootApplication`)
2. Right-click the file
3. Select **Run**

Or run via terminal:

```bash
mvn spring-boot:run
```

or via maven wrapper:

```bash
./mvnw clean install
``` 

---

## Application Class

This is the main entry point of a Spring Boot application.  
It is responsible for bootstrapping and starting the application context.

Location: `src/main/java/com/example/Application.java`

Example implementation:

```java
package com.example;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class Application {

  public static void main(String[] args) {
    SpringApplication.run(Application.class, args);
  }
}
```

### Explanation

- `@SpringBootApplication` is a meta-annotation that includes:
    - `@Configuration`
    - `@EnableAutoConfiguration`
    - `@ComponentScan`
- `SpringApplication.run(...)` starts the embedded server (Tomcat by default)
- This class must be located in a **root package** so component scanning works correctly

---

## ApplicationTests Class

This class is used to verify that the Spring application context loads successfully.

Location: `src/test/java/com/example/ApplicationTests.java`

Example implementation:

```java
package com.example;

import org.junit.jupiter.api.Test;
import org.springframework.boot.test.context.SpringBootTest;

@SpringBootTest
class ApplicationTests {

  @Test
  void contextLoads() {
  }
}
```

### Explanation

- `@SpringBootTest` loads the full Spring application context
- `contextLoads()` is a smoke test:
    - It passes if the application starts without errors
    - It fails if bean configuration or context initialization fails
- This test is usually the **first test** in a Spring Boot project

---

## When to Extend These Classes

- Add REST controllers, services, and repositories under the same base package
- Add more test methods or separate test classes for:
    - Controllers (`@WebMvcTest`)
    - Services (`@ExtendWith(MockitoExtension.class)`)
    - Integration tests (`@SpringBootTest` with profiles)

---

## Common IntelliJ Tips

- Green ▶️ icon next to `main` method → Run application
- Green ▶️ icon next to test class or method → Run tests
- `mvn test` → runs all tests from terminal

---

## Summary

- Maven uses `pom.xml` to manage the project
- IntelliJ IDEA provides built-in Maven support
- Standard directory structure is important
- Source roots are automatically detected
- Spring Boot applications can be run directly from IntelliJ

This setup is suitable for enterprise-grade Spring Boot projects and modular architectures.
