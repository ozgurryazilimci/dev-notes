# Spring Boot Testing Guide

This document explains **Unit Tests**, **Controller Tests**, and **Integration Tests** in Spring Boot,
including when and how to use common testing annotations.

---

## Test Types Overview

| Test Type        | Purpose                          | Spring Context      |
|------------------|----------------------------------|---------------------|
| Unit Test        | Test business logic in isolation | ❌ No Spring Context |
| Controller Test  | Test web layer (MVC) only        | ⚠️ Partial Context  |
| Integration Test | Test full application flow       | ✅ Full Context      |

---

## Unit Tests

### When to Use

- Testing service or component logic
- No Spring container needed
- Fast and isolated
- External dependencies are mocked

---

### Common Annotations

- `@ExtendWith(MockitoExtension.class)`
- `@Mock`
- `@InjectMocks`
- `@Test`

---

### Example Unit Test

Location: `src/test/java/com/example/service/OrderServiceTest.java`

```java
package com.example.service;

import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.extension.ExtendWith;
import org.mockito.InjectMocks;
import org.mockito.Mock;
import org.mockito.junit.jupiter.MockitoExtension;

import static org.mockito.Mockito.when;
import static org.assertj.core.api.Assertions.assertThat;

@ExtendWith(MockitoExtension.class)
class OrderServiceTest {

  @Mock
  private PaymentClient paymentClient;

  @InjectMocks
  private OrderService orderService;

  @Test
  void shouldCreateOrderSuccessfully() {
    when(paymentClient.pay()).thenReturn(true);

    boolean result = orderService.createOrder();

    assertThat(result).isTrue();
  }
}
```

---

### Key Points

- No `@SpringBootTest`
- No application context startup
- Fastest type of test
- Preferred for business logic

---

## Controller Tests (Web Layer Tests)

### When to Use

- Testing REST controllers
- Request/response validation
- HTTP status codes and JSON mapping
- Service layer is mocked

---

### Common Annotations

- `@WebMvcTest`
- `@MockBean`
- `@Test`
- `MockMvc`

---

### Example Controller Test

Location: `src/test/java/com/example/controller/OrderControllerTest.java`

```java
package com.example.controller;

import com.example.service.OrderService;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.autoconfigure.web.servlet.WebMvcTest;
import org.springframework.boot.test.mock.mockito.MockBean;
import org.springframework.test.web.servlet.MockMvc;

import static org.mockito.Mockito.when;
import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.get;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.status;

@WebMvcTest(OrderController.class)
class OrderControllerTest {

  @Autowired
  private MockMvc mockMvc;

  @MockBean
  private OrderService orderService;

  @Test
  void shouldReturnOk() throws Exception {
    when(orderService.isHealthy()).thenReturn(true);

    mockMvc.perform(get("/orders/health"))
           .andExpect(status().isOk());
  }
}
```

---

### Key Points

- Loads **only MVC components**
- Does NOT load full context
- Faster than integration tests
- Ideal for API contract validation

---

## Integration Tests

### When to Use

- Testing full application behavior
- Database, messaging, security, configuration
- Verifying beans wiring together correctly

---

### Common Annotations

- `@SpringBootTest`
- `@ActiveProfiles`
- `@Test`
- `@AutoConfigureMockMvc`

---

### Example Integration Test

Location: `src/test/java/com/example/integration/OrderIntegrationTest.java`

```java
package com.example.integration;

import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.autoconfigure.web.servlet.AutoConfigureMockMvc;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.test.web.servlet.MockMvc;

import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.get;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.status;

@SpringBootTest
@AutoConfigureMockMvc
class OrderIntegrationTest {

  @Autowired
  private MockMvc mockMvc;

  @Test
  void shouldLoadContextAndReturnOk() throws Exception {
    mockMvc.perform(get("/orders/health"))
           .andExpect(status().isOk());
  }
}
```

```java
package com.example.presentation.rest;

import com.example.application.order.OrderService;
import com.example.application.order.command.CreateOrderCommand;
import com.example.application.order.result.OrderResult;
import com.example.presentation.rest.OrderController;
import com.fasterxml.jackson.databind.ObjectMapper;
import java.math.BigDecimal;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.autoconfigure.web.servlet.WebMvcTest;
import org.springframework.boot.test.mock.mockito.MockBean;
import org.springframework.http.MediaType;
import org.springframework.test.web.servlet.MockMvc;

import static org.mockito.ArgumentMatchers.any;
import static org.mockito.Mockito.when;
import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.post;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.jsonPath;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.status;

@WebMvcTest(OrderController.class)
class OrderControllerTest {

  @Autowired
  private MockMvc mockMvc;

  @MockBean
  private OrderService orderService;

  @Autowired
  private ObjectMapper objectMapper;

  // ---------- CREATE ORDER TEST ----------

  @Test
  void should_create_order_successfully() throws Exception {
    // given
    CreateOrderCommand command = new CreateOrderCommand(
        "customer-123",
        BigDecimal.valueOf(250.0)
    );

    OrderResult result = new OrderResult(
        1L,
        "customer-123",
        BigDecimal.valueOf(250.0),
        "CREATED"
    );

    when(orderService.createOrder(any(CreateOrderCommand.class))).thenReturn(result);

    // when & then
    mockMvc.perform(post("/orders")
                        .contentType(MediaType.APPLICATION_JSON)
                        .content(objectMapper.writeValueAsString(command)))
           .andExpect(status().isOk())
           .andExpect(jsonPath("$.id").value(1L))
           .andExpect(jsonPath("$.status").value("CREATED"))
           .andExpect(jsonPath("$.customerId").value("customer-123"))
           .andExpect(jsonPath("$.amount").value(250.0));
  }
}
```

---

### Key Points

- Loads full Spring context
- Slowest but most realistic
- Should be limited in number
- Best for end-to-end verification

---

## Annotation Summary

| Annotation                            | Usage                              |
|---------------------------------------|------------------------------------|
| `@ExtendWith(MockitoExtension.class)` | Unit tests with Mockito            |
| `@Mock`                               | Mock a dependency                  |
| `@InjectMocks`                        | Inject mocks into class under test |
| `@WebMvcTest`                         | Controller (MVC) tests             |
| `@MockBean`                           | Mock Spring beans                  |
| `@SpringBootTest`                     | Full integration test              |

---

## Recommended Testing Strategy

- ✅ Mostly **Unit Tests**
- ✅ Some **Controller Tests**
- ⚠️ Few **Integration Tests**

This approach provides fast feedback while still ensuring application reliability.
