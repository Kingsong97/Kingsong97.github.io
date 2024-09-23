
# ToDo List - CRUD Basic

## 1. 프로젝트 개요
- **개발 환경**: IntelliJ IDEA
- **백엔드**: Java (Spring Boot), MySQL
- **빌드 도구**: Gradle
- **프론트엔드**: Thymeleaf (또는 React/Vue.js)

### 주요 기능
- ToDo 항목을 데이터베이스에 저장, 조회, 수정, 삭제 (CRUD)
- Spring Boot를 사용한 REST API 및 서버 템플릿 엔진(Thymeleaf) 활용

---

## 2. 프로젝트 설정 및 단계

### 2.1 Gradle 설정
```gradle
plugins {
    id 'java'
    id 'org.springframework.boot' version '3.3.3'
    id 'io.spring.dependency-management' version '1.1.6'
}

group = 'com.example'
version = '0.0.1-SNAPSHOT'

java {
    toolchain {
        languageVersion = JavaLanguageVersion.of(22)
    }
}

repositories {
    mavenCentral()
}

dependencies {
    implementation 'org.springframework.boot:spring-boot-starter-data-jdbc'
    implementation 'org.springframework.boot:spring-boot-starter-data-jpa'
    implementation 'org.springframework.boot:spring-boot-starter-web'
    compileOnly 'org.projectlombok:lombok'
    runtimeOnly 'com.mysql:mysql-connector-j'
    annotationProcessor 'org.projectlombok:lombok'
    testImplementation 'org.springframework.boot:spring-boot-starter-test'
}

tasks.named('test') {
    useJUnitPlatform()
}
```

### 2.2 MySQL 데이터베이스 설정
1. **MySQL 데이터베이스**: `TodoList` 데이터베이스 생성
```sql
CREATE DATABASE TodoList;
```

2. **`application.properties` 설정**:
```properties
spring.datasource.url=jdbc:mysql://localhost:3306/TodoList
spring.datasource.username=root
spring.datasource.password=1q2w3e4r!!
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver

spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.MySQLDialect
```

### 2.3 엔티티(Entity) 클래스
```java
package com.example.demo.todo.model;

import jakarta.persistence.*;
import lombok.Data;

@Entity
@Data
public class Todo {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String title;
    private String description;
    private boolean completed;
}
```

### 2.4 레포지토리(Repository) 인터페이스
```java
package com.example.demo.todo.repository;

import org.springframework.data.jpa.repository.JpaRepository;
import com.example.demo.todo.model.Todo;

public interface TodoRepository extends JpaRepository<Todo, Long> {
}
```

### 2.5 서비스(Service) 클래스
```java
package com.example.demo.todo.service;

import com.example.demo.todo.model.Todo;
import com.example.demo.todo.repository.TodoRepository;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import java.util.List;

@Service
public class TodoService {

    @Autowired
    private TodoRepository todoRepository;

    public List<Todo> getAllTodos() {
        return todoRepository.findAll();
    }

    public Todo saveTodo(Todo todo) {
        return todoRepository.save(todo);
    }
}
```

### 2.6 컨트롤러(Controller) 클래스
```java
package com.example.demo.todo.controller;

import com.example.demo.todo.model.Todo;
import com.example.demo.todo.service.TodoService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;

import java.util.List;

@Controller
public class TodoController {

    @Autowired
    private TodoService todoService;

    @GetMapping("/")
    public String getTodoList(Model model) {
        List<Todo> todos = todoService.getAllTodos();
        model.addAttribute("todos", todos);
        return "todo-list";
    }
}
```

### 2.7 Thymeleaf 템플릿 (HTML)
```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <title>ToDo List</title>
</head>
<body>
    <h1>My ToDo List</h1>
    <ul>
        <li th:each="todo : ${todos}">
            <span th:text="${todo.title}">Title</span> - 
            <span th:text="${todo.description}">Description</span>
        </li>
    </ul>
</body>
</html>
```

---

## 3. 트러블슈팅 (Troubleshooting)

### 3.1 Gradle 버전 문제
- **문제**: `Gradle 8.10.1 requires Java 1.8 or later to run. You are currently using Java 1.7.` 에러 발생.
- **해결**: Java 1.8 이상 버전을 설치하고, IntelliJ에서 JDK 설정을 Java 1.8 이상으로 변경.

### 3.2 데이터베이스 연결 문제
- **문제**: `Failed to configure a DataSource: 'url' attribute is not specified` 에러 발생.
- **해결**: `application.properties` 파일에서 `spring.datasource.url`, `username`, `password` 등의 설정이 올바르게 입력되었는지 확인.

### 3.3 캐시 삭제 후 프로젝트 설정
- **문제**: IntelliJ에서 `application.properties` 파일이 주황색으로 표시됨.
- **해결**: `src/main/resources` 폴더를 우클릭하고 `Mark Directory as > Resources Root`로 설정.

### 3.4 `PUT` 요청 시 Internal Server Error 발생
- **문제**: `PUT` 요청으로 데이터를 업데이트하려고 할 때, 500 에러가 발생하면서 `Name for argument of type [java.lang.Long] not specified` 오류 발생.
- **해결**: 컴파일러가 `-parameters` 플래그를 사용해 컴파일되었는지 확인. 또는 URL 패스 변수를 정확하게 지정해줌. @PathVariable을 사용하는 메서드에 인자를 정확히 넣었는지 확인.

### 3.5 환경 변수 설정 문제
- **문제**: `application.properties`에서 `${DB_URL}`, `${DB_USERNAME}` 등으로 환경 변수를 설정했을 때 값이 제대로 적용되지 않음.
- **해결**: IntelliJ 또는 IDE의 런타임 설정에서 환경 변수를 명시적으로 추가했는지 확인하거나 `env.properties` 파일을 생성해 필요한 설정을 담아두고, Spring의 `@PropertySource`로 파일을 불러와야 함.

### 3.6 GitHub Repository Push 문제
- **문제**: `No such file or directory` 에러 발생.
- **해결**: 명령어에 `git clone` 또는 `git push origin` 명령어 없이 URL을 바로 실행함. GitHub 명령어 규칙에 맞게 `git remote add origin <repository-url>`을 먼저 실행한 후 작업.

---
