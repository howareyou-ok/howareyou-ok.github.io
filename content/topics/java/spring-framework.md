---
title: "Spring Framework: Building Enterprise Java Applications"
date: 2024-01-18T11:00:00+08:00
tags: ["java", "spring", "framework", "enterprise"]
---

Spring Framework is the most popular Java framework for building enterprise applications. It provides comprehensive infrastructure support and promotes good design practices through dependency injection and aspect-oriented programming.

## What is Spring Framework?

Spring is a lightweight, open-source framework that makes Java development easier and more productive. It provides a comprehensive programming and configuration model for modern Java-based enterprise applications.

## Core Features

### Dependency Injection (DI)

Spring's core feature that manages object dependencies automatically:

```java
// Without Spring - Manual dependency management
public class OrderService {
    private PaymentService paymentService;
    private EmailService emailService;
    
    public OrderService() {
        this.paymentService = new PaymentService();
        this.emailService = new EmailService();
    }
}

// With Spring - Automatic dependency injection
@Service
public class OrderService {
    @Autowired
    private PaymentService paymentService;
    
    @Autowired
    private EmailService emailService;
    
    // Spring automatically injects dependencies
}
```

### Inversion of Control (IoC)

Spring container manages the lifecycle of objects:

```java
@Component
public class UserRepository {
    public User findById(Long id) {
        // Database logic here
        return new User(id, "John Doe");
    }
}

@Service
public class UserService {
    @Autowired
    private UserRepository userRepository;
    
    public User getUser(Long id) {
        return userRepository.findById(id);
    }
}
```

## Getting Started with Spring Boot

Spring Boot makes it easy to create stand-alone, production-grade Spring applications:

### Project Setup

Create a new Spring Boot project using Spring Initializr:

1. Visit [start.spring.io](https://start.spring.io)
2. Choose Maven/Gradle, Java version, and Spring Boot version
3. Add dependencies: Spring Web, Spring Data JPA, H2 Database
4. Generate and download the project

### Basic Application Structure

```java
@SpringBootApplication
public class Application {
    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}
```

### Creating a REST Controller

```java
@RestController
@RequestMapping("/api/users")
public class UserController {
    
    @Autowired
    private UserService userService;
    
    @GetMapping("/{id}")
    public ResponseEntity<User> getUser(@PathVariable Long id) {
        User user = userService.findById(id);
        return ResponseEntity.ok(user);
    }
    
    @PostMapping
    public ResponseEntity<User> createUser(@RequestBody User user) {
        User savedUser = userService.save(user);
        return ResponseEntity.status(HttpStatus.CREATED).body(savedUser);
    }
    
    @GetMapping
    public ResponseEntity<List<User>> getAllUsers() {
        List<User> users = userService.findAll();
        return ResponseEntity.ok(users);
    }
}
```

## Spring Data JPA

Simplify database operations with Spring Data JPA:

### Entity Class

```java
@Entity
@Table(name = "users")
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    @Column(nullable = false)
    private String name;
    
    @Column(unique = true, nullable = false)
    private String email;
    
    // Constructors, getters, and setters
    public User() {}
    
    public User(String name, String email) {
        this.name = name;
        this.email = email;
    }
    
    // Getters and setters...
}
```

### Repository Interface

```java
@Repository
public interface UserRepository extends JpaRepository<User, Long> {
    
    // Custom query methods
    List<User> findByName(String name);
    
    Optional<User> findByEmail(String email);
    
    @Query("SELECT u FROM User u WHERE u.name LIKE %:name%")
    List<User> findUsersWithNameContaining(@Param("name") String name);
}
```

### Service Layer

```java
@Service
@Transactional
public class UserService {
    
    @Autowired
    private UserRepository userRepository;
    
    public User save(User user) {
        return userRepository.save(user);
    }
    
    public User findById(Long id) {
        return userRepository.findById(id)
            .orElseThrow(() -> new UserNotFoundException("User not found with id: " + id));
    }
    
    public List<User> findAll() {
        return userRepository.findAll();
    }
    
    public void deleteById(Long id) {
        userRepository.deleteById(id);
    }
}
```

## Configuration

### Application Properties

```properties
# Database configuration
spring.datasource.url=jdbc:h2:mem:testdb
spring.datasource.driver-class-name=org.h2.Driver
spring.datasource.username=sa
spring.datasource.password=

# JPA configuration
spring.jpa.database-platform=org.hibernate.dialect.H2Dialect
spring.jpa.hibernate.ddl-auto=create-drop
spring.jpa.show-sql=true

# Server configuration
server.port=8080
```

### Java Configuration

```java
@Configuration
@EnableJpaRepositories
public class DatabaseConfig {
    
    @Bean
    public DataSource dataSource() {
        HikariConfig config = new HikariConfig();
        config.setJdbcUrl("jdbc:h2:mem:testdb");
        config.setUsername("sa");
        config.setPassword("");
        return new HikariDataSource(config);
    }
}
```

## Testing with Spring

### Unit Testing

```java
@ExtendWith(MockitoExtension.class)
class UserServiceTest {
    
    @Mock
    private UserRepository userRepository;
    
    @InjectMocks
    private UserService userService;
    
    @Test
    void shouldReturnUserWhenFound() {
        // Given
        Long userId = 1L;
        User expectedUser = new User("John Doe", "john@example.com");
        when(userRepository.findById(userId)).thenReturn(Optional.of(expectedUser));
        
        // When
        User actualUser = userService.findById(userId);
        
        // Then
        assertEquals(expectedUser.getName(), actualUser.getName());
        assertEquals(expectedUser.getEmail(), actualUser.getEmail());
    }
}
```

### Integration Testing

```java
@SpringBootTest
@AutoConfigureTestDatabase(replace = AutoConfigureTestDatabase.Replace.NONE)
class UserControllerIntegrationTest {
    
    @Autowired
    private TestRestTemplate restTemplate;
    
    @Autowired
    private UserRepository userRepository;
    
    @Test
    void shouldCreateUser() {
        // Given
        User newUser = new User("Jane Doe", "jane@example.com");
        
        // When
        ResponseEntity<User> response = restTemplate.postForEntity("/api/users", newUser, User.class);
        
        // Then
        assertEquals(HttpStatus.CREATED, response.getStatusCode());
        assertNotNull(response.getBody().getId());
        assertEquals("Jane Doe", response.getBody().getName());
    }
}
```

## Spring Security

Add security to your application:

### Dependencies

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-security</artifactId>
</dependency>
```

### Security Configuration

```java
@Configuration
@EnableWebSecurity
public class SecurityConfig {
    
    @Bean
    public PasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder();
    }
    
    @Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
        http
            .authorizeHttpRequests(authz -> authz
                .requestMatchers("/api/public/**").permitAll()
                .requestMatchers("/api/admin/**").hasRole("ADMIN")
                .anyRequest().authenticated()
            )
            .httpBasic(Customizer.withDefaults())
            .csrf(csrf -> csrf.disable());
        
        return http.build();
    }
}
```

## Best Practices

### Project Structure

```
src/
├── main/
│   ├── java/
│   │   └── com/example/app/
│   │       ├── Application.java
│   │       ├── controller/
│   │       ├── service/
│   │       ├── repository/
│   │       ├── model/
│   │       └── config/
│   └── resources/
│       ├── application.properties
│       └── static/
└── test/
    └── java/
        └── com/example/app/
```

### Coding Guidelines

1. **Use Constructor Injection**: Prefer constructor injection over field injection
2. **Keep Controllers Thin**: Business logic should be in service layer
3. **Use DTOs**: Don't expose entities directly in REST APIs
4. **Handle Exceptions**: Implement global exception handling
5. **Write Tests**: Maintain good test coverage

### Performance Tips

- Use connection pooling
- Implement caching with `@Cacheable`
- Use pagination for large datasets
- Optimize database queries
- Profile your application regularly

## Common Annotations

- `@SpringBootApplication`: Main application class
- `@RestController`: REST API controller
- `@Service`: Service layer component
- `@Repository`: Data access layer component
- `@Entity`: JPA entity
- `@Autowired`: Dependency injection
- `@RequestMapping`: URL mapping
- `@Transactional`: Transaction management

## Next Steps

After mastering Spring basics:

1. **Learn Spring Security**: Authentication and authorization
2. **Explore Spring Cloud**: Microservices architecture
3. **Study Spring WebFlux**: Reactive programming
4. **Master Spring Boot Actuator**: Production monitoring
5. **Practice with Real Projects**: Build complete applications

Spring Framework's extensive ecosystem and excellent documentation make it an ideal choice for Java enterprise development. Start with Spring Boot for rapid development, then gradually explore other Spring projects as your needs grow.