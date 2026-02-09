# Gradle Project Setup with IntelliJ IDEA

This page explains how to create and open a Gradle-based Spring Boot project in IntelliJ IDEA, how the standard project
structure looks, and includes an example `build.gradle`.

---

## What is Gradle?

Gradle is a modern build automation tool used for Java and JVM-based projects.  
It uses Groovy or Kotlin DSL scripts to define:

- Project metadata
- Dependencies
- Build logic
- Java version
- Plugins

Gradle is known for its performance, flexibility, and strong support for multi-module projects.

---

## Example `build.gradle`

Below is a sample **Gradle project** using Spring Boot and Java 21.

<details>
<summary>build.gradle</summary>

```groovy
plugins {
  id 'java'
  id 'org.springframework.boot' version '3.5.4'
  id 'io.spring.dependency-management' version '1.1.6'
}

group = 'com.example'
version = '1.0.0-SNAPSHOT'

java {
  toolchain {
    languageVersion = JavaLanguageVersion.of(21)
  }
}

repositories {
  mavenCentral()
}

dependencies {

  implementation 'org.springframework.boot:spring-boot-starter'
  implementation 'org.springframework.boot:spring-boot-starter-web'

  testImplementation 'org.springframework.boot:spring-boot-starter-test'
}

tasks.named('test') {
  useJUnitPlatform()
}
```

</details>

---

## Adding Additional Dependencies

Gradle allows you to add dependencies using the `dependencies` block inside `build.gradle`.

---

### Swagger (OpenAPI) Support

To enable Swagger UI and OpenAPI documentation, add the following dependency:

```groovy
dependencies {
  implementation 'org.springdoc:springdoc-openapi-starter-webmvc-ui:2.5.0'
}
```

Swagger UI and OpenAPI endpoints will be available at  
`/swagger-ui/index.html` and `/v3/api-docs`.

Legacy Swagger support using Springfox can be added with:

```groovy
plugins {
  id 'java'
  id 'org.springframework.boot' version '2.2.2.RELEASE'
  id 'io.spring.dependency-management' version '1.0.10.RELEASE'
}

group 'com.example'
version '1.0'

repositories {
  mavenCentral()
}

dependencies {

  // spring boot
  implementation 'org.springframework.boot:spring-boot-starter'
  implementation 'org.springframework.boot:spring-boot-starter-web'

  implementation 'io.springfox:springfox-swagger2:2.9.2'
  implementation 'io.springfox:springfox-swagger-ui:2.9.2'

  //test
  testImplementation('org.springframework.boot:spring-boot-starter-test') {
    exclude group: 'org.junit.vintage', module: 'junit-vintage-engine'
  }
}

test {
  useJUnitPlatform()
}
```

```java
package com.example.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import springfox.documentation.builders.PathSelectors;
import springfox.documentation.builders.RequestHandlerSelectors;
import springfox.documentation.spi.DocumentationType;
import springfox.documentation.spring.web.plugins.Docket;
import springfox.documentation.swagger2.annotations.EnableSwagger2;

@Configuration
@EnableSwagger2
public class SwaggerConfig {

  @Bean
  public Docket api() {
    return new Docket(DocumentationType.SWAGGER_2)
        .select()
        .apis(RequestHandlerSelectors.basePackage("com.example"))
        .paths(PathSelectors.any())
        .build();
  }
}
```

Then you can use `@Api(tags = "Orders")` and `@ApiOperation("Create a new order")` annotations in your controllers to
enhance the documentation.

Swagger UI and OpenAPI endpoints will be available at  
`/swagger-ui.html` and `/v3/api-docs`.

---

### Logging Support (SLF4J)

To add SLF4J API for logging abstraction:

```groovy
dependencies {
  implementation 'org.slf4j:slf4j-api'
}
```

Spring Boot already provides a default logging implementation (Logback), so no additional configuration is required.

---

## Opening a Gradle Project in IntelliJ IDEA

1. Open **IntelliJ IDEA**
2. Click **File → Open**
3. Select the folder that contains `build.gradle`
4. Click **OK**
5. IntelliJ will detect the Gradle project
6. Choose **Load Gradle Project** when prompted

Gradle dependencies will be downloaded automatically.

---

## Standard Gradle Project Structure

    sample-gradle-project
    ├── build.gradle
    ├── settings.gradle
    ├── gradlew
    ├── gradlew.bat
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

- `src/main/java` → **Sources Root**
- `src/main/resources` → **Resources Root**
- `src/test/java` → **Test Sources Root**
- `src/test/resources` → **Test Resources Root**

---

## Running the Application

### From IntelliJ IDEA

- Open the main class annotated with `@SpringBootApplication`
- Click the green ▶️ icon next to the `main` method

### From Terminal

```bash
./gradlew bootRun
```

---

## Application Class

Location: `src/main/java/com/example/Application.java`

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

---

## ApplicationTests Class

Location: `src/test/java/com/example/ApplicationTests.java`

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

---

## Gradle and Java Version Compatibility

| Gradle Version | Minimum Java Version | Maximum Java Version | Notes                                 |
|----------------|----------------------|----------------------|---------------------------------------|
| 6.0 – 6.9      | 8                    | 14                   | Java 14 requires Gradle 6.7+          |
| 7.0 – 7.5      | 8                    | 17                   | Gradle 7.3+ fully supports Java 17    |
| 8.0 – 8.x      | 8                    | 21                   | Latest Java 21 support                |
| 9.x (future)   | 11                   | TBD                  | Planned future support for newer Java |

### Notes:

- Gradle supports running on higher Java versions, but **some plugins or tasks may not work** if the Gradle version is
  too old.
- Always check the [Gradle Release Notes](https://docs.gradle.org/current/release-notes.html) for detailed Java
  compatibility.
- Spring Boot 3.x requires **Java 17+**, so Gradle 7.3+ is recommended for full support.

---

## Summary

- Gradle uses `build.gradle` instead of `pom.xml`
- IntelliJ IDEA provides native Gradle integration
- Project structure is identical to Maven
- Gradle Wrapper ensures build consistency
- Spring Boot works seamlessly with Gradle
