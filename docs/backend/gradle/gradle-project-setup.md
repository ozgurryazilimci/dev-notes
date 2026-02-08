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

## Summary

- Gradle uses `build.gradle` instead of `pom.xml`
- IntelliJ IDEA provides native Gradle integration
- Project structure is identical to Maven
- Gradle Wrapper ensures build consistency
- Spring Boot works seamlessly with Gradle
