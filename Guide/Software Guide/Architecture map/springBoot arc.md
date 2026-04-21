# Spring Boot App — Architecture Map
> Complete guide from project setup to production support

---

## Legend

| Symbol | Meaning |
|--------|---------|
| ✅ | Recommended |
| 🔵 | Alternative |
| ⭐ | Added — commonly missing |
| ⚠️ | Situational |
| ~~strikethrough~~ | Avoid |
| 🔴 | Legacy only |

---

## Table of Contents

- [Step 1 · Project Setup](#step-1--project-setup)
- [Step 2 · Language & Code Quality](#step-2--language--code-quality)
- [Step 3 · Architecture & Design Patterns](#step-3--architecture--design-patterns)
- [Step 4 · Data Access Layer](#step-4--data-access-layer)
- [Step 5 · API Design](#step-5--api-design)
- [Step 6 · Security](#step-6--security)
- [Step 7 · Messaging & Events ⭐](#step-7--messaging--events-)
- [Step 8 · Caching ⭐](#step-8--caching-)
- [Step 9 · Resilience & Fault Tolerance ⭐](#step-9--resilience--fault-tolerance-)
- [Step 10 · Validation & Error Handling](#step-10--validation--error-handling)
- [Step 11 · Testing](#step-11--testing)
- [Step 12 · Observability ⭐](#step-12--observability-)
- [Step 13 · CI/CD & Deployment](#step-13--cicd--deployment)
- [Step 14 · Performance ⭐](#step-14--performance-)
- [Step 15 · Configuration & Secrets Management ⭐](#step-15--configuration--secrets-management-)
- [Step 16 · Documentation & DX ⭐](#step-16--documentation--dx-)
- [Quick Reference — Full Recommended Stack](#quick-reference--full-recommended-stack)

---

## Step 1 · Project Setup
> The build tool, runtime, and project initialiser that powers your app

### In this step

**⚙️ Build Tools** — compile, package, and manage dependencies
- Maven ✅
- Gradle ✅

**☕ Java Versions** — which JDK to use and why
- Java 21 ✅
- Java 17 🔵

**🚀 Project Bootstrapping** — fastest way to create a new Spring Boot project
- Spring Initializr ✅
- Spring Boot CLI 🔵

**🏗️ Runtimes & Packaging** — how the app is packaged and run
- Embedded Tomcat ✅
- Embedded Undertow 🔵
- Native Image (GraalVM) ⭐
- Executable JAR ✅
- WAR deployment 🔴

---

### ⚙️ Build Tools

**Maven** ✅
- Convention-over-configuration — standard directory layout, predictable lifecycle
- `pom.xml` — declarative dependency and plugin management
- Best for: teams familiar with XML, enterprise orgs with existing Maven infrastructure

```xml
<!-- pom.xml -->
<parent>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-parent</artifactId>
  <version>3.3.0</version>
</parent>

<dependencies>
  <dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
  </dependency>
</dependencies>
```

**Gradle** ✅
- Faster incremental builds than Maven — especially in large multi-module projects
- `build.gradle` (Groovy) or `build.gradle.kts` (Kotlin DSL — preferred for type safety)
- Best for: Kotlin projects, Android teams, projects needing custom build logic

```kotlin
// build.gradle.kts
plugins {
  id("org.springframework.boot") version "3.3.0"
  id("io.spring.dependency-management") version "1.1.5"
  kotlin("jvm") version "1.9.24"
}

dependencies {
  implementation("org.springframework.boot:spring-boot-starter-web")
}
```

### ☕ Java Versions

| Version | Status | Notes |
|---------|--------|-------|
| **Java 21** | ✅ | LTS — virtual threads (Project Loom), pattern matching, records, sealed classes |
| **Java 17** | 🔵 | LTS — still widely used in enterprise, records, sealed classes |
| Java 11 | 🔴 | End of mainstream support — migrate away |
| Java 8  | 🔴 | Very legacy — avoid for new projects |

> Spring Boot 3.x requires Java 17 minimum. Use Java 21 for new projects to get virtual threads.

### 🚀 Project Bootstrapping

**Spring Initializr** ✅ — https://start.spring.io
- Select dependencies, Java version, build tool, packaging
- Download as ZIP or generate directly via IDE (IntelliJ, VS Code)

```bash
# via curl
curl https://start.spring.io/starter.zip \
  -d dependencies=web,data-jpa,security,actuator \
  -d javaVersion=21 \
  -d type=gradle-project \
  -o my-app.zip
```

### 🏗️ Runtimes & Packaging

| Option | Status | Use When |
|--------|--------|----------|
| **Embedded Tomcat** | ✅ | Default — battle-tested, zero config |
| **Embedded Undertow** | 🔵 | Lower memory footprint, better for reactive |
| **GraalVM Native Image** | ⭐ | Instant startup, low memory — great for serverless / Lambda |
| **Executable JAR** | ✅ | Standard deployment unit — `java -jar app.jar` |
| WAR deployment | 🔴 | Legacy — only if deploying to an external app server |

[↑ Back to top](#table-of-contents)

---

## Step 2 · Language & Code Quality
> Tools that catch mistakes, enforce standards, and reduce boilerplate

### In this step

**🔤 Language Choices** — Java vs Kotlin on the JVM
- Java 21 ✅
- Kotlin ⭐

**📉 Boilerplate Reduction** — less code, same functionality
- Lombok ✅
- Kotlin data classes ⭐
- Java Records ⭐

**🔍 Static Analysis & Linting** — catch bugs and style issues before runtime
- Checkstyle ✅
- SpotBugs ⭐
- PMD 🔵
- SonarQube ⭐

**🚦 Git Hooks & CI Checks** — block bad code before it enters the repo
- pre-commit hooks ✅
- Spotless (auto-format) ⭐

---

### 🔤 Language Choices

**Java 21** ✅
- Records — immutable data classes in one line
- Sealed classes — exhaustive type hierarchies
- Pattern matching — cleaner instanceof checks
- Virtual threads (Project Loom) — massive concurrency with simple blocking code

```java
// Record — replaces a full POJO with getters, equals, hashCode, toString
public record UserDto(Long id, String email, String role) {}

// Sealed class — only these subtypes allowed
public sealed interface PaymentResult
  permits PaymentResult.Success, PaymentResult.Failure {}

// Pattern matching
if (result instanceof PaymentResult.Success s) {
  return s.transactionId();
}
```

**Kotlin** ⭐
- Null safety built into the type system — `String` vs `String?`
- Extension functions, coroutines, data classes
- Fully interoperable with Java — use any Java library
- Spring Boot has first-class Kotlin support

```kotlin
// Null safety
fun greet(name: String?): String =
  name?.uppercase() ?: "GUEST"

// Data class — equals, hashCode, toString, copy for free
data class UserDto(val id: Long, val email: String, val role: String)

// Extension function
fun String.toSlug() = lowercase().replace(" ", "-")
```

### 📉 Boilerplate Reduction

**Lombok** ✅

```java
@Data               // getters, setters, equals, hashCode, toString
@Builder            // builder pattern
@NoArgsConstructor
@AllArgsConstructor
public class User {
  private Long id;
  private String email;
  private String role;
}

// Usage
User user = User.builder().id(1L).email("alice@example.com").build();
```

> Caution: Lombok generates code invisibly — can surprise new team members. Consider Java Records for immutable DTOs instead.

**Java Records** ⭐

```java
// Replaces Lombok @Data for immutable DTOs — built into the language
public record CreateUserRequest(
  @NotBlank String email,
  @NotBlank String password,
  @NotNull Role role
) {}
```

### 🔍 Static Analysis & Linting

| Tool | Status | Purpose |
|------|--------|---------|
| **Checkstyle** | ✅ | Enforce code style (indentation, naming, line length) |
| **SpotBugs** | ⭐ | Find common bug patterns — null dereferences, resource leaks |
| **PMD** | 🔵 | Unused variables, empty catch blocks, complex methods |
| **SonarQube** | ⭐ | Comprehensive code quality and security scanning — the gold standard in enterprise |

```xml
<!-- Maven — add to pom.xml -->
<plugin>
  <groupId>org.apache.maven.plugins</groupId>
  <artifactId>maven-checkstyle-plugin</artifactId>
  <version>3.3.1</version>
</plugin>
```

### 🚦 Git Hooks & Auto-formatting

**Spotless** ⭐ — auto-formats Java/Kotlin code on build

```kotlin
// build.gradle.kts
plugins { id("com.diffplug.spotless") version "6.25.0" }

spotless {
  java { googleJavaFormat() }
  kotlin { ktlint() }
}
```

Run `./gradlew spotlessApply` before committing. Add `spotlessCheck` to CI to block unformatted code.

[↑ Back to top](#table-of-contents)

---

## Step 3 · Architecture & Design Patterns
> How your codebase is organised and how components interact

### In this step

**📁 Application Architecture** — how to structure the codebase at the macro level
- Layered (N-tier) Architecture ✅
- Hexagonal (Ports & Adapters) Architecture ⭐
- Package by feature ⭐
- Package by layer 🔵

**🧩 Core Spring Patterns** — the building blocks Spring Boot gives you
- Dependency Injection (DI) ✅
- Controller / Service / Repository pattern ✅
- Component scanning ✅
- Bean lifecycle and scopes ⭐

**🏗️ Design Patterns** — reusable solutions to common problems
- Builder pattern ✅
- Factory pattern ⭐
- Strategy pattern ⭐
- Observer pattern ⭐
- Decorator pattern ⭐
- Template method pattern ⭐

**🔀 Data Transfer Patterns** — how data moves between layers
- DTO (Data Transfer Object) ✅
- Mapper pattern (MapStruct) ✅
- Command / Query separation ⭐

---

### 📁 Application Architecture

#### ✅ Layered Architecture — standard for most Spring Boot apps

```
src/main/java/com/example/myapp/
├── controller/         ← HTTP layer — handles requests and responses
│     └── UserController.java
├── service/            ← Business logic layer
│     └── UserService.java
├── repository/         ← Data access layer
│     └── UserRepository.java
├── domain/             ← Entities and domain objects
│     └── User.java
├── dto/                ← Data Transfer Objects
│     ├── UserDto.java
│     └── CreateUserRequest.java
├── mapper/             ← DTO ↔ Entity conversion
│     └── UserMapper.java
├── config/             ← Spring configuration classes
│     └── SecurityConfig.java
└── exception/          ← Custom exceptions and handlers
      └── GlobalExceptionHandler.java
```

#### ⭐ Package by Feature — recommended for large apps

Instead of grouping by technical layer, group by business domain. Everything for a feature lives together.

```
src/main/java/com/example/myapp/
├── user/
│     ├── UserController.java
│     ├── UserService.java
│     ├── UserRepository.java
│     ├── User.java            ← entity
│     ├── UserDto.java
│     └── UserMapper.java
├── order/
│     ├── OrderController.java
│     ├── OrderService.java
│     ├── OrderRepository.java
│     └── Order.java
└── shared/
      ├── exception/
      └── config/
```

**Why package by feature for large teams:** each team owns a package end-to-end; changes to one feature rarely touch another package; easier to extract into a microservice later.

#### ⭐ Hexagonal Architecture (Ports & Adapters)

Separates the core business logic from all external concerns (HTTP, database, messaging). The domain has no dependency on Spring or any framework.

```
src/main/java/com/example/
├── domain/                      ← pure business logic, no framework imports
│     ├── model/User.java
│     ├── port/in/CreateUserUseCase.java    ← interface the app exposes
│     └── port/out/UserRepository.java     ← interface the domain needs
├── application/                 ← use case implementations
│     └── CreateUserService.java
└── adapter/
      ├── in/web/UserController.java       ← HTTP adapter
      ├── out/persistence/                 ← JPA adapter
      │     ├── UserJpaRepository.java
      │     └── UserPersistenceAdapter.java
      └── out/messaging/                   ← Kafka adapter
            └── UserEventPublisher.java
```

> Use for: microservices where the domain must be testable without infrastructure, or apps that might swap databases or message brokers.

---

### 🧩 Core Spring Patterns

#### Dependency Injection ✅

```java
// Constructor injection — preferred (immutable, testable)
@Service
public class OrderService {
  private final OrderRepository orderRepository;
  private final PaymentService paymentService;

  public OrderService(OrderRepository orderRepository, PaymentService paymentService) {
    this.orderRepository = orderRepository;
    this.paymentService  = paymentService;
  }
}

// ❌ Field injection — avoid (hidden dependencies, hard to test)
@Service
public class OrderService {
  @Autowired private OrderRepository orderRepository; // BAD
}
```

#### Controller / Service / Repository ✅

```java
// Controller — HTTP only, no business logic
@RestController
@RequestMapping("/api/users")
public class UserController {
  private final UserService userService;

  @GetMapping("/{id}")
  public ResponseEntity<UserDto> getUser(@PathVariable Long id) {
    return ResponseEntity.ok(userService.findById(id));
  }
}

// Service — business logic only, no HTTP or DB concerns
@Service
@Transactional
public class UserService {
  private final UserRepository userRepository;
  private final UserMapper userMapper;

  public UserDto findById(Long id) {
    return userRepository.findById(id)
      .map(userMapper::toDto)
      .orElseThrow(() -> new UserNotFoundException(id));
  }
}

// Repository — data access only
@Repository
public interface UserRepository extends JpaRepository<User, Long> {
  Optional<User> findByEmail(String email);
}
```

#### Bean Scopes ⭐

| Scope | Meaning | Use When |
|-------|---------|----------|
| `singleton` | One instance per Spring context (default) | Stateless services, repositories |
| `prototype` | New instance every time | Stateful beans (rare) |
| `request` | One instance per HTTP request | Request-scoped data (user context) |
| `session` | One instance per HTTP session | Session-scoped data |

---

### 🏗️ Design Patterns

#### Strategy Pattern ⭐

Define a family of algorithms, make them interchangeable.

```java
// Interface — the strategy contract
public interface PaymentStrategy {
  PaymentResult pay(PaymentRequest request);
}

// Concrete strategies
@Component("CARD")   public class CardPaymentStrategy   implements PaymentStrategy { ... }
@Component("UPI")    public class UpiPaymentStrategy    implements PaymentStrategy { ... }
@Component("WALLET") public class WalletPaymentStrategy implements PaymentStrategy { ... }

// Context — selects strategy at runtime
@Service
public class PaymentService {
  private final Map<String, PaymentStrategy> strategies;

  public PaymentResult process(String method, PaymentRequest req) {
    return strategies.getOrDefault(method, strategies.get("CARD")).pay(req);
  }
}
```

#### Factory Pattern ⭐

```java
@Component
public class NotificationFactory {
  private final Map<NotificationType, NotificationSender> senders;

  public NotificationSender getSender(NotificationType type) {
    return Optional.ofNullable(senders.get(type))
      .orElseThrow(() -> new IllegalArgumentException("No sender for: " + type));
  }
}
```

#### Decorator Pattern ⭐

Add behaviour to a service without modifying it.

```java
// Base service
@Service
public class UserServiceImpl implements UserService { ... }

// Decorator — adds caching on top
@Primary
@Service
public class CachingUserService implements UserService {
  private final UserService delegate;
  private final Cache cache;

  public UserDto findById(Long id) {
    return cache.get(id, () -> delegate.findById(id));
  }
}
```

---

### 🔀 Data Transfer Patterns

#### DTO Pattern ✅

Never expose JPA entities directly in API responses — they can trigger lazy loading, expose internal fields, and couple your API to your database schema.

```java
// Entity — internal, for persistence
@Entity
public class User {
  @Id @GeneratedValue private Long id;
  private String email;
  @JsonIgnore private String passwordHash; // should never leave the backend
  @OneToMany private List<Order> orders;   // lazy — don't expose directly
}

// DTO — external, safe for the API
public record UserDto(Long id, String email, String role) {}
```

#### MapStruct ✅

Auto-generates type-safe mapper implementations at compile time — no reflection, no runtime cost.

```bash
# Maven dependency
<dependency>
  <groupId>org.mapstruct</groupId>
  <artifactId>mapstruct</artifactId>
  <version>1.5.5.Final</version>
</dependency>
```

```java
@Mapper(componentModel = "spring")
public interface UserMapper {
  UserDto toDto(User user);
  User toEntity(CreateUserRequest request);
  List<UserDto> toDtoList(List<User> users);
}
```

#### Command / Query Separation (CQRS) ⭐

Separate read operations (queries) from write operations (commands). Simplest form — just two service methods with clear naming.

```java
// Commands — change state, return void or minimal result
userCommandService.createUser(CreateUserCommand command);
userCommandService.updateEmail(UpdateEmailCommand command);
userCommandService.deleteUser(Long userId);

// Queries — read state, no side effects
UserDto        userQueryService.findById(Long id);
Page<UserDto>  userQueryService.search(UserSearchCriteria criteria);
List<UserDto>  userQueryService.findByRole(Role role);
```

[↑ Back to top](#table-of-contents)

---

## Step 4 · Data Access Layer
> How your app reads from and writes to the database

### In this step

**💡 Concepts** — data access patterns that matter in production
- N+1 query problem and how to solve it ⭐
- Pagination — never return unbounded lists ⭐
- Projections — fetch only what you need ⭐
- Optimistic vs pessimistic locking ⭐
- Database migrations with Flyway ✅

**🗄️ ORM & Data Access Tools** — how to talk to relational databases
- Spring Data JPA ✅
- Hibernate ✅
- QueryDSL ⭐
- JOOQ ⭐
- Spring Data JDBC 🔵

**🗃️ Database Options** — relational and non-relational
- PostgreSQL ✅
- MySQL / MariaDB 🔵
- H2 (testing only) ✅
- MongoDB ⭐
- Redis ⭐

**🔄 Migration Tools** — manage schema changes safely
- Flyway ✅
- Liquibase 🔵

---

### 💡 Concepts

#### The N+1 Query Problem ⭐

The most common performance bug in JPA applications. Loading 100 orders and then issuing 1 query per order to load the user = 101 queries instead of 1.

```java
// ❌ N+1 — 1 query for orders + 1 query per order for user
List<Order> orders = orderRepository.findAll();
orders.forEach(o -> System.out.println(o.getUser().getName())); // triggers lazy load

// ✅ Fetch join — 1 query total
@Query("SELECT o FROM Order o JOIN FETCH o.user WHERE o.status = :status")
List<Order> findByStatusWithUser(@Param("status") OrderStatus status);

// ✅ Entity graph — declarative eager loading
@EntityGraph(attributePaths = {"user", "items"})
List<Order> findAll();
```

#### Pagination ⭐

Never return an unbounded list of records from an API. Always paginate.

```java
// Repository — Spring Data handles the SQL
Page<User> findByRole(Role role, Pageable pageable);

// Service
public Page<UserDto> findUsers(Role role, int page, int size) {
  Pageable pageable = PageRequest.of(page, size, Sort.by("createdAt").descending());
  return userRepository.findByRole(role, pageable).map(userMapper::toDto);
}

// Controller
@GetMapping
public ResponseEntity<Page<UserDto>> list(
  @RequestParam(defaultValue = "0") int page,
  @RequestParam(defaultValue = "20") int size
) {
  return ResponseEntity.ok(userService.findUsers(Role.USER, page, size));
}
```

#### Projections ⭐

Fetch only the columns you need instead of loading the entire entity.

```java
// Interface projection — Spring Data generates the query
public interface UserSummary {
  Long getId();
  String getEmail();
}

List<UserSummary> findAllProjectedBy();

// DTO projection — JPQL constructor expression
@Query("SELECT new com.example.dto.UserSummary(u.id, u.email) FROM User u")
List<UserSummary> findSummaries();
```

#### Optimistic Locking ⭐

Prevent lost updates when two transactions modify the same row concurrently.

```java
@Entity
public class Product {
  @Id private Long id;
  private Integer stock;

  @Version
  private Long version;  // auto-incremented on every update
  // If two transactions update the same version, second one throws OptimisticLockException
}
```

#### Database Migrations with Flyway ✅

Every schema change goes through a migration file. Never alter the database manually.

```
src/main/resources/db/migration/
  V1__create_users_table.sql
  V2__add_role_to_users.sql
  V3__create_orders_table.sql
  V4__add_index_on_email.sql
```

```sql
-- V1__create_users_table.sql
CREATE TABLE users (
  id         BIGSERIAL PRIMARY KEY,
  email      VARCHAR(255) NOT NULL UNIQUE,
  password   VARCHAR(255) NOT NULL,
  role       VARCHAR(50)  NOT NULL DEFAULT 'USER',
  created_at TIMESTAMP    NOT NULL DEFAULT NOW()
);
```

---

### 🗄️ ORM & Data Access Tools

#### Spring Data JPA ✅

```java
public interface UserRepository extends JpaRepository<User, Long> {
  // Spring Data generates SQL from method names
  Optional<User> findByEmail(String email);
  List<User> findByRoleAndActiveTrue(Role role);
  boolean existsByEmail(String email);

  // Custom JPQL
  @Query("SELECT u FROM User u WHERE u.createdAt > :date AND u.role = :role")
  List<User> findRecentByRole(@Param("date") LocalDateTime date, @Param("role") Role role);

  // Native SQL
  @Query(value = "SELECT * FROM users WHERE email ILIKE %:term%", nativeQuery = true)
  List<User> searchByEmail(@Param("term") String term);
}
```

#### QueryDSL ⭐

Type-safe dynamic queries — no string concatenation, compile-time checked.

```java
QUser user = QUser.user;

List<User> result = queryFactory
  .selectFrom(user)
  .where(
    user.role.eq(Role.ADMIN),
    user.createdAt.after(LocalDateTime.now().minusDays(30)),
    user.email.containsIgnoreCase(searchTerm)
  )
  .orderBy(user.createdAt.desc())
  .offset(pageable.getOffset())
  .limit(pageable.getPageSize())
  .fetch();
```

#### JOOQ ⭐

Generates Java code from your database schema. SQL-first approach — full control over queries.

```java
// Generated from schema — type-safe, refactor-safe
Result<Record> result = dsl
  .select(USERS.ID, USERS.EMAIL, ORDERS.TOTAL)
  .from(USERS)
  .join(ORDERS).on(USERS.ID.eq(ORDERS.USER_ID))
  .where(ORDERS.STATUS.eq("PENDING"))
  .orderBy(ORDERS.CREATED_AT.desc())
  .limit(20)
  .fetch();
```

> Use JOOQ when you want full SQL control and don't want Hibernate abstracting your queries away.

---

### 🗃️ Database Options

| Database | Status | Use When |
|----------|--------|----------|
| **PostgreSQL** | ✅ | Default choice — ACID, JSON support, extensions, free |
| **MySQL / MariaDB** | 🔵 | Widely used, good tooling, familiar to most teams |
| **H2** | ✅ (test only) | In-memory database for unit/integration tests |
| **MongoDB** | ⭐ | Document store — flexible schema, nested data, Spring Data MongoDB |
| **Redis** | ⭐ | Key-value — caching, sessions, rate limiting, pub/sub |

---

### 🔄 Migration Tools

| Tool | Status | Use When |
|------|--------|----------|
| **Flyway** | ✅ | SQL-first, simple, most widely used |
| **Liquibase** | 🔵 | XML/YAML/JSON changesets, better rollback support |

[↑ Back to top](#table-of-contents)

---

## Step 5 · API Design
> How your app exposes its functionality to the outside world

### In this step

**💡 Concepts** — API design patterns every production API needs
- RESTful resource naming ✅
- HTTP status codes — using them correctly ✅
- API versioning strategies ⭐
- Pagination and filtering conventions ⭐
- Standard error response format ⭐
- HATEOAS ⭐

**🛠️ Tools** — build and expose APIs
- Spring MVC (REST) ✅
- Spring WebFlux (Reactive) 🔵
- GraphQL (Spring for GraphQL) ⭐
- gRPC ⭐
- OpenAPI / Swagger ✅

---

### 💡 Concepts

#### RESTful Resource Naming ✅

```
# Nouns, not verbs. Plural resources.
GET    /api/users           → list users
POST   /api/users           → create user
GET    /api/users/{id}      → get user
PUT    /api/users/{id}      → replace user
PATCH  /api/users/{id}      → partial update
DELETE /api/users/{id}      → delete user

# Nested resources for relationships
GET    /api/users/{id}/orders        → user's orders
POST   /api/users/{id}/orders        → create order for user
GET    /api/users/{id}/orders/{oid}  → specific order

# ❌ Verbs in URL — wrong
POST /api/createUser
GET  /api/getUserById
POST /api/deleteUser
```

#### HTTP Status Codes ✅

```
200 OK            → successful GET, PUT, PATCH
201 Created       → successful POST — include Location header
204 No Content    → successful DELETE
400 Bad Request   → validation failed — include field errors
401 Unauthorized  → not authenticated
403 Forbidden     → authenticated but not authorised
404 Not Found     → resource does not exist
409 Conflict      → duplicate email, optimistic lock failure
422 Unprocessable → business rule violation
500 Internal Error → unexpected server error
```

#### API Versioning Strategies ⭐

| Strategy | Example | When to use |
|----------|---------|-------------|
| **URI versioning** | `/api/v1/users` | Most common, explicit, easy to route |
| **Header versioning** | `Accept: application/vnd.app.v1+json` | Cleaner URLs, harder to test in browser |
| **Query param** | `/api/users?version=1` | Avoid — pollutes query params |

```java
// URI versioning — recommended
@RestController
@RequestMapping("/api/v1/users")
public class UserControllerV1 { ... }

@RestController
@RequestMapping("/api/v2/users")
public class UserControllerV2 { ... }
```

#### Standard Error Response Format ⭐

Consistent error responses across all endpoints — clients know exactly what to expect.

```java
// Error response DTO
public record ApiError(
  int    status,
  String error,
  String message,
  String path,
  Instant timestamp,
  List<FieldError> fieldErrors  // for validation failures
) {}

public record FieldError(String field, String message) {}
```

```json
{
  "status": 400,
  "error": "Bad Request",
  "message": "Validation failed",
  "path": "/api/v1/users",
  "timestamp": "2024-01-15T10:30:00Z",
  "fieldErrors": [
    { "field": "email", "message": "must be a valid email address" },
    { "field": "password", "message": "must be at least 8 characters" }
  ]
}
```

#### Pagination Convention ⭐

```
GET /api/users?page=0&size=20&sort=createdAt,desc

Response:
{
  "content": [...],
  "page": {
    "size": 20,
    "totalElements": 243,
    "totalPages": 13,
    "number": 0
  }
}
```

---

### 🛠️ Tools

#### Spring MVC ✅

```java
@RestController
@RequestMapping("/api/v1/users")
@RequiredArgsConstructor
public class UserController {

  private final UserService userService;

  @GetMapping
  public ResponseEntity<Page<UserDto>> list(
    @RequestParam(defaultValue = "0") int page,
    @RequestParam(defaultValue = "20") int size
  ) {
    return ResponseEntity.ok(userService.findAll(page, size));
  }

  @PostMapping
  public ResponseEntity<UserDto> create(@Valid @RequestBody CreateUserRequest request) {
    UserDto created = userService.create(request);
    URI location = URI.create("/api/v1/users/" + created.id());
    return ResponseEntity.created(location).body(created);
  }

  @DeleteMapping("/{id}")
  public ResponseEntity<Void> delete(@PathVariable Long id) {
    userService.delete(id);
    return ResponseEntity.noContent().build();
  }
}
```

#### Spring WebFlux (Reactive) 🔵

```java
@RestController
@RequestMapping("/api/users")
public class UserController {

  @GetMapping
  public Flux<UserDto> list() {
    return userService.findAll();
  }

  @GetMapping("/{id}")
  public Mono<ResponseEntity<UserDto>> get(@PathVariable Long id) {
    return userService.findById(id)
      .map(ResponseEntity::ok)
      .defaultIfEmpty(ResponseEntity.notFound().build());
  }
}
```

> Use WebFlux for: high-concurrency I/O-bound workloads, streaming responses, or when integrating with reactive data stores (R2DBC, MongoDB Reactive).

#### OpenAPI / Swagger ✅

```xml
<dependency>
  <groupId>org.springdoc</groupId>
  <artifactId>springdoc-openapi-starter-webmvc-ui</artifactId>
  <version>2.5.0</version>
</dependency>
```

```java
@Operation(summary = "Get user by ID")
@ApiResponses({
  @ApiResponse(responseCode = "200", description = "User found"),
  @ApiResponse(responseCode = "404", description = "User not found")
})
@GetMapping("/{id}")
public ResponseEntity<UserDto> getById(@PathVariable Long id) { ... }
```

Access at: `http://localhost:8080/swagger-ui.html`

#### Spring for GraphQL ⭐

```java
@Controller
public class UserGraphQLController {

  @QueryMapping
  public UserDto user(@Argument Long id) {
    return userService.findById(id);
  }

  @MutationMapping
  public UserDto createUser(@Argument CreateUserInput input) {
    return userService.create(input);
  }

  @SchemaMapping(typeName = "User")
  public List<OrderDto> orders(UserDto user) {
    return orderService.findByUserId(user.id());
  }
}
```

> Use GraphQL when clients need flexible queries, or when one endpoint needs to serve many different frontend shapes (mobile vs web vs partner API).

[↑ Back to top](#table-of-contents)

---

## Step 6 · Security
> Protecting your API and its data

### In this step

**💡 Concepts** — security patterns every production app needs
- Authentication vs Authorisation ✅
- JWT — how it works and its trade-offs ✅
- OAuth2 / OIDC — delegating auth to an identity provider ⭐
- Method-level security ⭐
- CORS configuration ✅
- CSRF — when it matters and when it doesn't ✅

**🛠️ Tools** — Spring Security and related libraries
- Spring Security ✅
- JWT (jjwt / nimbus-jose-jwt) ✅
- Spring Security OAuth2 Resource Server ⭐
- Keycloak ⭐

---

### 💡 Concepts

#### Authentication vs Authorisation

```
Authentication — who are you?        → login, JWT, OAuth2
Authorisation  — what can you do?   → roles, permissions, method security
```

#### JWT Flow ✅

```
1. Client sends credentials to POST /auth/login
2. Server validates, returns { accessToken, refreshToken }
3. Client stores accessToken (memory) and refreshToken (httpOnly cookie)
4. Client sends Authorization: Bearer <accessToken> on every request
5. Server validates JWT signature and claims — no database lookup needed
6. When accessToken expires, client uses refreshToken to get a new one
```

#### OAuth2 / OIDC ⭐

Delegate authentication to a trusted Identity Provider (IdP) — Google, Okta, Keycloak, Auth0.

```
1. User clicks "Login with Google"
2. App redirects to Google with client_id and redirect_uri
3. User authenticates with Google
4. Google redirects back with an authorization code
5. App exchanges code for access_token + id_token (PKCE for SPAs)
6. App validates the JWT and creates a session
```

> For enterprise apps, Keycloak or Okta is the standard — your app becomes a resource server, not an auth server.

---

### 🛠️ Tools

#### Spring Security ✅

```java
@Configuration
@EnableWebSecurity
@EnableMethodSecurity
public class SecurityConfig {

  @Bean
  public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
    return http
      .csrf(AbstractHttpConfigurer::disable)          // stateless API — no CSRF needed
      .sessionManagement(s -> s.sessionCreationPolicy(STATELESS))
      .authorizeHttpRequests(auth -> auth
        .requestMatchers("/api/auth/**", "/actuator/health").permitAll()
        .requestMatchers("/api/admin/**").hasRole("ADMIN")
        .anyRequest().authenticated()
      )
      .oauth2ResourceServer(oauth2 -> oauth2.jwt(Customizer.withDefaults()))
      .build();
  }
}
```

#### Method-level Security ⭐

```java
@Service
public class OrderService {

  @PreAuthorize("hasRole('ADMIN') or #userId == authentication.principal.id")
  public List<OrderDto> findByUser(Long userId) { ... }

  @PreAuthorize("hasAuthority('orders:delete')")
  public void delete(Long orderId) { ... }

  @PostAuthorize("returnObject.userId == authentication.principal.id")
  public OrderDto findById(Long id) { ... }
}
```

#### JWT with Spring Security Resource Server ⭐

```yaml
# application.yml
spring:
  security:
    oauth2:
      resourceserver:
        jwt:
          issuer-uri: https://your-idp.com/realms/myrealm
          # Spring auto-fetches public keys from the JWKS endpoint
```

#### Keycloak ⭐

Open-source Identity Provider — handles login, registration, MFA, social login, RBAC, and token issuance. Your Spring Boot app just validates the JWT it receives.

```yaml
spring:
  security:
    oauth2:
      resourceserver:
        jwt:
          issuer-uri: http://localhost:8180/realms/myapp
```

#### Password Hashing ✅

```java
@Bean
public PasswordEncoder passwordEncoder() {
  return new BCryptPasswordEncoder(12); // cost factor 12 — ~250ms per hash
}

// Usage
String hash = passwordEncoder.encode(rawPassword);
boolean matches = passwordEncoder.matches(rawPassword, hash);
```

> Never store plain text passwords. Never use MD5 or SHA-1. BCrypt or Argon2 only.

[↑ Back to top](#table-of-contents)

---

## Step 7 · Messaging & Events ⭐
> Decoupling services with asynchronous communication — missing from most guides

### In this step

**💡 Concepts** — event-driven patterns and when to use them
- Synchronous vs asynchronous communication ⭐
- Event-driven architecture ⭐
- Transactional Outbox pattern ⭐
- Dead letter queues ⭐
- Idempotent consumers ⭐

**🛠️ Tools** — messaging infrastructure
- Apache Kafka ✅
- RabbitMQ 🔵
- Spring Events (in-process) ✅
- AWS SQS / SNS ⭐

---

### 💡 Concepts

#### Synchronous vs Asynchronous ⭐

```
Synchronous  → caller waits for the response
  Use for: queries, reads, operations that need an immediate result

Asynchronous → caller fires and forgets, processes continue independently
  Use for: sending emails, generating reports, updating downstream systems,
           anything that should not block the user response
```

#### Transactional Outbox Pattern ⭐

The hardest problem in event-driven systems: what if you save to the database but the message broker is down? The outbox pattern guarantees at-least-once delivery.

```java
@Service
@Transactional
public class OrderService {

  public Order createOrder(CreateOrderCommand cmd) {
    Order order = orderRepository.save(new Order(cmd));

    // Write event to outbox table in SAME transaction as the order
    // If this transaction commits, the event WILL be published eventually
    // If it rolls back, neither the order nor the event is saved
    outboxRepository.save(new OutboxEvent(
      "ORDER_CREATED",
      objectMapper.writeValueAsString(new OrderCreatedEvent(order.getId()))
    ));

    return order;
  }
}

// Separate scheduler reads outbox and publishes to Kafka
@Scheduled(fixedDelay = 1000)
public void publishPendingEvents() {
  outboxRepository.findUnpublished().forEach(event -> {
    kafkaTemplate.send("orders", event.getPayload());
    event.markPublished();
  });
}
```

#### Idempotent Consumers ⭐

Messages can be delivered more than once. Consumers must handle duplicates.

```java
@KafkaListener(topics = "orders")
public void handleOrderCreated(OrderCreatedEvent event) {
  // Check if already processed
  if (processedEventRepository.existsById(event.getEventId())) {
    return; // duplicate — skip
  }

  processOrder(event);

  // Mark as processed
  processedEventRepository.save(new ProcessedEvent(event.getEventId()));
}
```

#### Dead Letter Queues ⭐

When a consumer fails after retries, route the message to a DLQ instead of losing it.

```java
@Bean
public ConcurrentKafkaListenerContainerFactory<String, String> kafkaListenerContainerFactory() {
  factory.setCommonErrorHandler(new DefaultErrorHandler(
    new DeadLetterPublishingRecoverer(kafkaTemplate), // send to DLQ after retries
    new FixedBackOff(1000L, 3)                        // retry 3 times, 1s apart
  ));
  return factory;
}
```

---

### 🛠️ Tools

#### Apache Kafka ✅

```xml
<dependency>
  <groupId>org.springframework.kafka</groupId>
  <artifactId>spring-kafka</artifactId>
</dependency>
```

```java
// Producer
@Service
public class OrderEventPublisher {
  private final KafkaTemplate<String, OrderCreatedEvent> kafkaTemplate;

  public void publish(OrderCreatedEvent event) {
    kafkaTemplate.send("order-created", event.orderId().toString(), event);
  }
}

// Consumer
@Component
public class OrderCreatedConsumer {

  @KafkaListener(topics = "order-created", groupId = "inventory-service")
  public void handle(OrderCreatedEvent event) {
    inventoryService.reserveStock(event.items());
  }
}
```

```yaml
spring:
  kafka:
    bootstrap-servers: localhost:9092
    consumer:
      group-id: my-service
      auto-offset-reset: earliest
      key-deserializer: org.apache.kafka.common.serialization.StringDeserializer
      value-deserializer: org.springframework.kafka.support.serializer.JsonDeserializer
    producer:
      key-serializer: org.apache.kafka.common.serialization.StringSerializer
      value-serializer: org.springframework.kafka.support.serializer.JsonSerializer
```

#### RabbitMQ 🔵

```java
@Component
public class OrderConsumer {

  @RabbitListener(queues = "order.created")
  public void handle(OrderCreatedEvent event) {
    inventoryService.reserveStock(event.items());
  }
}
```

#### Spring Application Events ✅

For in-process async communication — same JVM only. No external broker needed.

```java
// Publish
applicationEventPublisher.publishEvent(new OrderCreatedEvent(order));

// Listen
@EventListener
@Async
public void onOrderCreated(OrderCreatedEvent event) {
  emailService.sendOrderConfirmation(event.order());
}
```

[↑ Back to top](#table-of-contents)

---

## Step 8 · Caching ⭐
> Reduce database load and improve response times — missing from most guides

### In this step

**💡 Concepts** — caching patterns and pitfalls
- Cache-aside pattern ✅
- Cache invalidation strategies ⭐
- Cache stampede / thundering herd ⭐
- What to cache vs what not to cache ⭐

**🛠️ Tools** — Spring Cache abstraction and backends
- Spring Cache ✅
- Redis (via Spring Data Redis) ✅
- Caffeine (local in-memory cache) ✅
- Multi-level caching ⭐

---

### 💡 Concepts

#### What to Cache vs What Not to Cache ⭐

```
✅ Cache:
  - Reference data (product catalogue, config, country list)
  - Expensive queries (reports, aggregations)
  - User session data
  - API responses from external services

❌ Don't cache:
  - Data that must always be fresh (account balance, stock levels)
  - Data that changes on every request
  - Large objects (puts memory pressure on Redis)
  - Sensitive data without encryption
```

#### Cache Invalidation Strategies ⭐

```
TTL (Time-To-Live)   → expire automatically after N seconds — simple, may serve stale data
Evict on write       → remove cache entry when the underlying data changes — always fresh
Write-through        → update cache and DB at the same time — complex, consistent
Cache-aside          → load on miss, evict on update — most common in Spring
```

#### Cache Stampede ⭐

When a popular cache entry expires, hundreds of requests simultaneously hit the database. Solutions: probabilistic early expiration, request coalescing, or a short TTL jitter.

```java
// Add jitter to TTL to avoid all entries expiring at the same time
int ttl = BASE_TTL + random.nextInt(60); // base + up to 60s jitter
```

---

### 🛠️ Tools

#### Spring Cache Abstraction ✅

```java
@Service
public class ProductService {

  @Cacheable(value = "products", key = "#id")
  public ProductDto findById(Long id) {
    return productRepository.findById(id)
      .map(productMapper::toDto)
      .orElseThrow(() -> new ProductNotFoundException(id));
  }

  @CacheEvict(value = "products", key = "#id")
  public void update(Long id, UpdateProductRequest request) {
    // cache entry removed after update
  }

  @CacheEvict(value = "products", allEntries = true)
  @Scheduled(fixedDelay = 3_600_000)
  public void evictAllProductsCache() {} // hourly full eviction
}
```

#### Redis ✅

```xml
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
```

```yaml
spring:
  data:
    redis:
      host: localhost
      port: 6379
  cache:
    type: redis
    redis:
      time-to-live: 600000  # 10 minutes in ms
```

#### Caffeine (local cache) ✅

In-memory cache — no network hop, microsecond latency. Use for data that is the same for all instances (reference data, config).

```yaml
spring:
  cache:
    type: caffeine
    caffeine:
      spec: maximumSize=1000,expireAfterWrite=10m
```

> Multi-level caching: Caffeine (L1, per-instance) → Redis (L2, shared). Check local cache first, fall back to Redis, then database.

[↑ Back to top](#table-of-contents)

---

## Step 9 · Resilience & Fault Tolerance ⭐
> Keeping your app stable when things go wrong — missing from most guides

### In this step

**💡 Concepts** — resilience patterns every distributed system needs
- Circuit Breaker — fail fast instead of waiting ⭐
- Retry — transient failures are normal ⭐
- Bulkhead — isolate failures ⭐
- Rate Limiter — protect your service ⭐
- Timeout — never wait forever ⭐
- Fallback — degrade gracefully ⭐

**🛠️ Tools** — resilience libraries for Spring Boot
- Resilience4j ✅
- Spring Retry ✅

---

### 💡 Concepts

#### Circuit Breaker ⭐

When a downstream service starts failing, stop sending requests to it. Give it time to recover. This prevents cascading failures.

```
CLOSED   → requests flow normally, failures counted
           (when failure rate > threshold)
OPEN     → all requests fail immediately — no calls to downstream
           (after wait duration)
HALF-OPEN → limited requests allowed through to test if service recovered
           (if success → CLOSED, if failure → OPEN again)
```

#### Retry ⭐

Transient failures (network blips, momentary overload) often resolve on the next attempt.

```
Retry only on: network timeouts, 503 Service Unavailable, connection refused
Never retry: 400 Bad Request, 401 Unauthorized, 404 Not Found — these won't change
Use exponential backoff with jitter: 100ms → 200ms → 400ms + random jitter
```

#### Bulkhead ⭐

Limit the number of concurrent calls to a downstream service. If it's slow, it won't drain your entire thread pool.

---

### 🛠️ Tools

#### Resilience4j ✅

```xml
<dependency>
  <groupId>io.github.resilience4j</groupId>
  <artifactId>resilience4j-spring-boot3</artifactId>
  <version>2.2.0</version>
</dependency>
```

```yaml
resilience4j:
  circuitbreaker:
    instances:
      payment-service:
        failure-rate-threshold: 50          # open when 50% of calls fail
        wait-duration-in-open-state: 30s
        sliding-window-size: 10
  retry:
    instances:
      payment-service:
        max-attempts: 3
        wait-duration: 500ms
        exponential-backoff-multiplier: 2
        retry-exceptions:
          - java.net.ConnectException
          - java.util.concurrent.TimeoutException
  timelimiter:
    instances:
      payment-service:
        timeout-duration: 3s
```

```java
@Service
public class PaymentClient {

  @CircuitBreaker(name = "payment-service", fallbackMethod = "paymentFallback")
  @Retry(name = "payment-service")
  @TimeLimiter(name = "payment-service")
  public CompletableFuture<PaymentResult> charge(ChargeRequest request) {
    return CompletableFuture.supplyAsync(() -> paymentApi.charge(request));
  }

  // Called when circuit is open or all retries exhausted
  public CompletableFuture<PaymentResult> paymentFallback(ChargeRequest request, Exception e) {
    return CompletableFuture.completedFuture(PaymentResult.queued("Payment queued for retry"));
  }
}
```

#### Spring Retry ✅

```java
@Retryable(
  retryFor = { ConnectException.class, TimeoutException.class },
  maxAttempts = 3,
  backoff = @Backoff(delay = 500, multiplier = 2)
)
public InvoiceDto fetchInvoice(Long invoiceId) {
  return invoiceApiClient.get(invoiceId);
}

@Recover
public InvoiceDto fetchInvoiceFallback(Exception e, Long invoiceId) {
  log.error("Failed to fetch invoice {} after retries", invoiceId, e);
  return InvoiceDto.empty(invoiceId);
}
```

[↑ Back to top](#table-of-contents)

---

## Step 10 · Validation & Error Handling
> Rejecting bad input early and returning useful error messages

### In this step

**💡 Concepts** — validation and error handling patterns
- Validate at the boundary — controller layer ✅
- Custom validators ⭐
- Global exception handling ✅
- Business exceptions vs technical exceptions ⭐
- Problem Details (RFC 7807) ⭐

**🛠️ Tools**
- Bean Validation (Jakarta Validation) ✅
- Spring's @ControllerAdvice ✅

---

### 💡 Concepts

#### Validate at the Boundary ✅

Validate input as close to the entry point as possible. Never pass invalid data into the service layer.

```java
public record CreateUserRequest(
  @NotBlank(message = "Email is required")
  @Email(message = "Must be a valid email")
  String email,

  @NotBlank
  @Size(min = 8, max = 100, message = "Password must be 8–100 characters")
  String password,

  @NotNull
  @Past(message = "Date of birth must be in the past")
  LocalDate dateOfBirth,

  @Valid                          // cascade validation to nested objects
  @NotNull
  AddressRequest address
) {}

// Controller — @Valid triggers validation, MethodArgumentNotValidException thrown on failure
@PostMapping
public ResponseEntity<UserDto> create(@Valid @RequestBody CreateUserRequest request) { ... }
```

#### Custom Validators ⭐

```java
// Custom annotation
@Target({FIELD, PARAMETER})
@Retention(RUNTIME)
@Constraint(validatedBy = UniqueEmailValidator.class)
public @interface UniqueEmail {
  String message() default "Email already registered";
  Class<?>[] groups() default {};
  Class<? extends Payload>[] payload() default {};
}

// Validator implementation
@Component
public class UniqueEmailValidator implements ConstraintValidator<UniqueEmail, String> {
  private final UserRepository userRepository;

  @Override
  public boolean isValid(String email, ConstraintValidatorContext context) {
    return email != null && !userRepository.existsByEmail(email);
  }
}
```

#### Global Exception Handler ✅

One place to handle all exceptions — consistent error responses across the entire API.

```java
@RestControllerAdvice
public class GlobalExceptionHandler {

  @ExceptionHandler(MethodArgumentNotValidException.class)
  public ResponseEntity<ApiError> handleValidation(
    MethodArgumentNotValidException ex,
    HttpServletRequest request
  ) {
    List<FieldError> errors = ex.getBindingResult().getFieldErrors().stream()
      .map(e -> new FieldError(e.getField(), e.getDefaultMessage()))
      .toList();

    return ResponseEntity.badRequest().body(new ApiError(
      400, "Bad Request", "Validation failed",
      request.getRequestURI(), Instant.now(), errors
    ));
  }

  @ExceptionHandler(ResourceNotFoundException.class)
  public ResponseEntity<ApiError> handleNotFound(
    ResourceNotFoundException ex,
    HttpServletRequest request
  ) {
    return ResponseEntity.status(404).body(new ApiError(
      404, "Not Found", ex.getMessage(),
      request.getRequestURI(), Instant.now(), List.of()
    ));
  }

  @ExceptionHandler(Exception.class)
  public ResponseEntity<ApiError> handleGeneral(Exception ex, HttpServletRequest request) {
    log.error("Unhandled exception at {}", request.getRequestURI(), ex);
    return ResponseEntity.status(500).body(new ApiError(
      500, "Internal Server Error", "An unexpected error occurred",
      request.getRequestURI(), Instant.now(), List.of()
    ));
  }
}
```

#### Business vs Technical Exceptions ⭐

```java
// Business exceptions — expected situations, meaningful HTTP codes
public class UserNotFoundException     extends RuntimeException { } // → 404
public class EmailAlreadyExistsException extends RuntimeException { } // → 409
public class InsufficientStockException  extends RuntimeException { } // → 422

// Technical exceptions — unexpected, always 500
// Log them fully. Never expose stack traces to clients.
```

[↑ Back to top](#table-of-contents)

---

## Step 11 · Testing
> Making sure the app works before it reaches users

### In this step

**💡 Concepts** — testing philosophy and strategy
- Test pyramid for Spring Boot ⭐
- Test behaviour, not implementation ⭐
- Test slices — test only the layer you care about ⭐

**🔬 Unit Testing** — fast, isolated tests for business logic
- JUnit 5 ✅
- Mockito ✅
- AssertJ ✅

**🔗 Integration Testing** — test the full slice with real dependencies
- @SpringBootTest ✅
- @DataJpaTest ✅
- @WebMvcTest ✅
- Testcontainers ⭐

**🌐 API & Contract Testing** — test the HTTP layer and API contracts
- MockMvc ✅
- RestAssured 🔵
- Spring Cloud Contract ⭐
- WireMock ⭐

---

### 💡 Concepts

#### Test Pyramid for Spring Boot ⭐

```
Few    → @SpringBootTest E2E — full application context, slow
Some   → @WebMvcTest / @DataJpaTest — slice tests, medium speed
Many   → Unit tests with Mockito — no Spring context, fast
```

#### Test Slices ⭐

Spring Boot test slices load only the part of the application context you need — much faster than `@SpringBootTest`.

| Annotation | Loads | Use For |
|------------|-------|---------|
| `@WebMvcTest` | Web layer only | Controller tests, request/response validation |
| `@DataJpaTest` | JPA layer only | Repository tests, query validation |
| `@JsonTest` | Jackson only | DTO serialisation / deserialisation |
| `@SpringBootTest` | Full context | E2E integration tests |

---

### 🔬 Unit Testing

```java
@ExtendWith(MockitoExtension.class)
class UserServiceTest {

  @Mock    private UserRepository userRepository;
  @Mock    private UserMapper userMapper;
  @InjectMocks private UserService userService;

  @Test
  void findById_returnsUserDto_whenUserExists() {
    // Arrange
    User user    = new User(1L, "alice@example.com", Role.USER);
    UserDto dto  = new UserDto(1L, "alice@example.com", "USER");
    given(userRepository.findById(1L)).willReturn(Optional.of(user));
    given(userMapper.toDto(user)).willReturn(dto);

    // Act
    UserDto result = userService.findById(1L);

    // Assert
    assertThat(result.email()).isEqualTo("alice@example.com");
    then(userRepository).should().findById(1L);
  }

  @Test
  void findById_throwsNotFoundException_whenUserMissing() {
    given(userRepository.findById(99L)).willReturn(Optional.empty());
    assertThatThrownBy(() -> userService.findById(99L))
      .isInstanceOf(UserNotFoundException.class);
  }
}
```

---

### 🔗 Integration Testing

#### @WebMvcTest — Controller layer

```java
@WebMvcTest(UserController.class)
class UserControllerTest {

  @Autowired MockMvc mockMvc;
  @MockBean  UserService userService;

  @Test
  void getUser_returns200_withUserDto() throws Exception {
    given(userService.findById(1L)).willReturn(new UserDto(1L, "alice@example.com", "USER"));

    mockMvc.perform(get("/api/v1/users/1")
        .contentType(MediaType.APPLICATION_JSON))
      .andExpect(status().isOk())
      .andExpect(jsonPath("$.email").value("alice@example.com"))
      .andExpect(jsonPath("$.role").value("USER"));
  }

  @Test
  void createUser_returns400_whenEmailInvalid() throws Exception {
    String body = """{"email": "not-an-email", "password": "secret123"}""";

    mockMvc.perform(post("/api/v1/users")
        .contentType(MediaType.APPLICATION_JSON)
        .content(body))
      .andExpect(status().isBadRequest())
      .andExpect(jsonPath("$.fieldErrors[0].field").value("email"));
  }
}
```

#### @DataJpaTest — Repository layer

```java
@DataJpaTest
class UserRepositoryTest {

  @Autowired UserRepository userRepository;

  @Test
  void findByEmail_returnsUser_whenEmailExists() {
    userRepository.save(new User("alice@example.com", "hash", Role.USER));

    Optional<User> result = userRepository.findByEmail("alice@example.com");

    assertThat(result).isPresent();
    assertThat(result.get().getEmail()).isEqualTo("alice@example.com");
  }
}
```

#### Testcontainers ⭐

Run real databases, Kafka, Redis in Docker during tests — no mocking of infrastructure.

```xml
<dependency>
  <groupId>org.testcontainers</groupId>
  <artifactId>postgresql</artifactId>
  <scope>test</scope>
</dependency>
```

```java
@SpringBootTest
@Testcontainers
class OrderServiceIntegrationTest {

  @Container
  static PostgreSQLContainer<?> postgres = new PostgreSQLContainer<>("postgres:16");

  @Container
  static KafkaContainer kafka = new KafkaContainer(DockerImageName.parse("confluentinc/cp-kafka:7.6.0"));

  @DynamicPropertySource
  static void properties(DynamicPropertyRegistry registry) {
    registry.add("spring.datasource.url", postgres::getJdbcUrl);
    registry.add("spring.kafka.bootstrap-servers", kafka::getBootstrapServers);
  }

  @Test
  void createOrder_publishesOrderCreatedEvent() { ... }
}
```

#### WireMock ⭐

Stub external HTTP APIs in tests — no real network calls.

```java
@SpringBootTest
@AutoConfigureWireMock(port = 0)
class PaymentClientTest {

  @Test
  void charge_returnsSuccess_whenApiResponds200() {
    stubFor(post(urlEqualTo("/payments/charge"))
      .willReturn(aResponse()
        .withStatus(200)
        .withHeader("Content-Type", "application/json")
        .withBody("""{"transactionId": "txn_123", "status": "SUCCESS"}""")));

    PaymentResult result = paymentClient.charge(new ChargeRequest(100L, "USD"));

    assertThat(result.status()).isEqualTo("SUCCESS");
  }
}
```

[↑ Back to top](#table-of-contents)

---

## Step 12 · Observability ⭐
> Seeing what is happening in production — missing from most guides

### In this step

**💡 Concepts** — the three pillars of observability
- Logs — what happened ✅
- Metrics — how the system is performing ✅
- Traces — where time was spent across services ⭐
- Structured logging ⭐
- Correlation IDs across services ⭐

**🛠️ Tools** — observability stack for Spring Boot
- Spring Boot Actuator ✅
- Micrometer ✅
- Prometheus + Grafana ✅
- OpenTelemetry ⭐
- Zipkin / Jaeger ⭐
- ELK Stack (Elasticsearch + Logstash + Kibana) 🔵
- Loki + Grafana ⭐

---

### 💡 Concepts

#### The Three Pillars ⭐

```
Logs    → what happened and when
          "2024-01-15T10:30:00Z ERROR OrderService - Payment failed for order 123: timeout"

Metrics → aggregated numbers over time
          "error rate: 0.3%, p99 latency: 245ms, active requests: 47"

Traces  → the journey of one request across multiple services
          "GET /orders/123 → OrderService (12ms) → PaymentService (89ms) → DB (3ms) = 104ms total"
```

#### Structured Logging ⭐

Log as JSON — searchable, filterable, machine-readable.

```java
// ❌ Unstructured — hard to search
log.info("User " + userId + " created order " + orderId + " for amount " + amount);

// ✅ Structured — every field is indexed
log.info("Order created",
  kv("userId", userId),
  kv("orderId", orderId),
  kv("amount", amount),
  kv("currency", currency)
);
```

```json
{
  "timestamp": "2024-01-15T10:30:00Z",
  "level": "INFO",
  "service": "order-service",
  "traceId": "abc123",
  "spanId": "def456",
  "message": "Order created",
  "userId": 42,
  "orderId": 789,
  "amount": 1500.00,
  "currency": "INR"
}
```

#### Correlation IDs ⭐

Propagate a unique request ID through all services so you can trace a request across log files.

```java
@Component
public class CorrelationIdFilter extends OncePerRequestFilter {

  @Override
  protected void doFilterInternal(HttpServletRequest req, HttpServletResponse res, FilterChain chain)
    throws ServletException, IOException {
    String correlationId = Optional.ofNullable(req.getHeader("X-Correlation-Id"))
      .orElse(UUID.randomUUID().toString());

    MDC.put("correlationId", correlationId);
    res.setHeader("X-Correlation-Id", correlationId);
    try {
      chain.doFilter(req, res);
    } finally {
      MDC.clear();
    }
  }
}
```

---

### 🛠️ Tools

#### Spring Boot Actuator ✅

```xml
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```

```yaml
management:
  endpoints:
    web:
      exposure:
        include: health,info,metrics,prometheus
  endpoint:
    health:
      show-details: when-authorized
```

Endpoints: `/actuator/health`, `/actuator/metrics`, `/actuator/prometheus`

#### Micrometer + Prometheus + Grafana ✅

```xml
<dependency>
  <groupId>io.micrometer</groupId>
  <artifactId>micrometer-registry-prometheus</artifactId>
</dependency>
```

```java
// Custom business metric
@Service
public class OrderService {
  private final Counter orderCreatedCounter;
  private final Timer orderProcessingTimer;

  public OrderService(MeterRegistry registry) {
    this.orderCreatedCounter  = registry.counter("orders.created");
    this.orderProcessingTimer = registry.timer("orders.processing.time");
  }

  public Order createOrder(CreateOrderCommand cmd) {
    return orderProcessingTimer.record(() -> {
      Order order = processOrder(cmd);
      orderCreatedCounter.increment();
      return order;
    });
  }
}
```

#### OpenTelemetry + Distributed Tracing ⭐

```xml
<dependency>
  <groupId>io.micrometer</groupId>
  <artifactId>micrometer-tracing-bridge-otel</artifactId>
</dependency>
<dependency>
  <groupId>io.opentelemetry</groupId>
  <artifactId>opentelemetry-exporter-otlp</artifactId>
</dependency>
```

```yaml
management:
  tracing:
    sampling:
      probability: 1.0   # 100% in dev, 10% in prod
  otlp:
    tracing:
      endpoint: http://jaeger:4318/v1/traces
```

Every incoming request automatically gets a `traceId` and `spanId` propagated through HTTP headers, Kafka messages, and log output.

[↑ Back to top](#table-of-contents)

---

## Step 13 · CI/CD & Deployment
> Getting code to production automatically and running it reliably

### In this step

**💡 Concepts** — environments, release cycle, and hotfix workflow
- The 3 environments (dev, staging, production) ✅
- Blue-green deployment ⭐
- Canary deployment ⭐
- Health checks and readiness probes ⭐

**⚙️ CI/CD Tools** — automate build, test, and deploy
- GitHub Actions ✅
- Jenkins 🔵
- GitLab CI 🔵

**📦 Containerisation** — package and run consistently anywhere
- Docker ✅
- Docker Compose ✅
- Kubernetes (K8s) ⭐
- Helm ⭐

**☁️ Cloud Platforms** — where the app runs
- AWS (ECS / EKS) ✅
- GCP (GKE) 🔵
- Azure (AKS) 🔵

---

### 💡 Concepts

#### The 3 Environments

```
DEVELOPMENT         STAGING              PRODUCTION
localhost:8080      staging.yourapp.com  yourapp.com
Only you see it     Team + QA sees it    Real users
H2 in-memory DB     Same infra as prod   Real database
DEBUG logging       INFO logging         WARN logging
```

#### Health Checks and Readiness Probes ⭐

Kubernetes uses these to know if a pod is alive and ready to receive traffic.

```yaml
# application.yml
management:
  endpoint:
    health:
      probes:
        enabled: true    # enables /actuator/health/liveness and /actuator/health/readiness

# kubernetes deployment.yaml
livenessProbe:
  httpGet:
    path: /actuator/health/liveness
    port: 8080
  initialDelaySeconds: 30
  periodSeconds: 10

readinessProbe:
  httpGet:
    path: /actuator/health/readiness
    port: 8080
  initialDelaySeconds: 15
  periodSeconds: 5
```

---

### ⚙️ CI/CD Pipeline

#### GitHub Actions ✅

```yaml
# .github/workflows/ci.yml
name: CI/CD

on: [push, pull_request]

jobs:
  build-and-test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-java@v4
        with: { java-version: '21', distribution: 'temurin' }

      - name: Run tests
        run: ./gradlew test

      - name: Static analysis
        run: ./gradlew spotbugsMain checkstyleMain

      - name: Build JAR
        run: ./gradlew bootJar

      - name: Build Docker image
        run: docker build -t myapp:${{ github.sha }} .

      - name: Push to registry
        run: docker push myapp:${{ github.sha }}

      - name: Deploy to staging
        if: github.ref == 'refs/heads/main'
        run: kubectl set image deployment/myapp myapp=myapp:${{ github.sha }}
```

---

### 📦 Containerisation

#### Dockerfile ✅

```dockerfile
# Multi-stage build — small final image
FROM eclipse-temurin:21-jdk AS builder
WORKDIR /app
COPY . .
RUN ./gradlew bootJar --no-daemon

FROM eclipse-temurin:21-jre
WORKDIR /app
COPY --from=builder /app/build/libs/*.jar app.jar

# Non-root user — security best practice
RUN addgroup --system appgroup && adduser --system --ingroup appgroup appuser
USER appuser

EXPOSE 8080
ENTRYPOINT ["java", "-jar", "app.jar"]
```

#### Docker Compose ✅

```yaml
# docker-compose.yml — for local development
services:
  app:
    build: .
    ports: ["8080:8080"]
    environment:
      SPRING_DATASOURCE_URL: jdbc:postgresql://db:5432/myapp
      SPRING_KAFKA_BOOTSTRAP_SERVERS: kafka:9092
    depends_on: [db, kafka]

  db:
    image: postgres:16
    environment: { POSTGRES_DB: myapp, POSTGRES_PASSWORD: secret }
    volumes: [postgres_data:/var/lib/postgresql/data]

  kafka:
    image: confluentinc/cp-kafka:7.6.0
    ports: ["9092:9092"]

volumes:
  postgres_data:
```

#### Kubernetes Deployment ⭐

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: myapp
spec:
  replicas: 3
  selector:
    matchLabels: { app: myapp }
  template:
    metadata:
      labels: { app: myapp }
    spec:
      containers:
        - name: myapp
          image: myapp:1.0.0
          ports: [{ containerPort: 8080 }]
          resources:
            requests: { memory: "256Mi", cpu: "250m" }
            limits:   { memory: "512Mi", cpu: "500m" }
          env:
            - name: SPRING_DATASOURCE_PASSWORD
              valueFrom:
                secretKeyRef: { name: db-secret, key: password }
          livenessProbe:
            httpGet: { path: /actuator/health/liveness,  port: 8080 }
          readinessProbe:
            httpGet: { path: /actuator/health/readiness, port: 8080 }
```

[↑ Back to top](#table-of-contents)

---

## Step 14 · Performance ⭐
> Making the app fast and keeping it fast under load — missing from most guides

### In this step

**💡 Concepts** — performance patterns for Spring Boot
- Virtual threads (Project Loom) ⭐
- Connection pool sizing ⭐
- Async processing ⭐
- Database query optimisation ✅
- JVM tuning basics ⭐

**🛠️ Tools** — profiling and load testing
- JProfiler / async-profiler ⭐
- Gatling ⭐
- JMeter 🔵
- Spring Boot Actuator metrics ✅

---

### 💡 Concepts

#### Virtual Threads (Project Loom) ⭐

Java 21 virtual threads replace the traditional one-thread-per-request model with lightweight virtual threads managed by the JVM. Your existing blocking code becomes massively concurrent with zero code changes.

```yaml
# application.yml — enable virtual threads (Spring Boot 3.2+)
spring:
  threads:
    virtual:
      enabled: true
```

```
Before virtual threads:
  - 200 platform threads → 200 concurrent requests max
  - Threads blocked on DB/HTTP waste OS resources

With virtual threads:
  - Millions of virtual threads → massive concurrency
  - Blocked virtual thread parks itself, freeing the carrier thread
  - Your blocking JDBC code is now as efficient as reactive code
```

#### Connection Pool Sizing ⭐

HikariCP is the default connection pool in Spring Boot. Sizing it correctly matters.

```yaml
spring:
  datasource:
    hikari:
      maximum-pool-size: 20        # rule of thumb: (num_cores * 2) + num_spindles
      minimum-idle: 5
      connection-timeout: 30000    # 30 seconds
      idle-timeout: 600000         # 10 minutes
      max-lifetime: 1800000        # 30 minutes
      leak-detection-threshold: 60000  # warn if connection held > 60 seconds
```

> Too many connections hurt performance — each PostgreSQL connection uses ~5-10MB RAM on the DB server.

#### Async Processing ⭐

Offload non-critical work from the request thread.

```java
@Service
public class NotificationService {

  @Async
  public CompletableFuture<Void> sendWelcomeEmail(User user) {
    emailClient.send(user.getEmail(), "Welcome!", buildTemplate(user));
    return CompletableFuture.completedFuture(null);
  }
}

// Configure the thread pool
@Bean
public Executor asyncExecutor() {
  ThreadPoolTaskExecutor executor = new ThreadPoolTaskExecutor();
  executor.setCorePoolSize(10);
  executor.setMaxPoolSize(50);
  executor.setQueueCapacity(100);
  executor.setThreadNamePrefix("async-");
  executor.initialize();
  return executor;
}
```

---

### 🛠️ Tools

#### Gatling ⭐ — load testing

```scala
// LoadTest.scala
class UserApiLoadTest extends Simulation {
  val httpProtocol = http.baseUrl("http://localhost:8080")

  val scenario = scenario("Get User")
    .exec(http("GET /api/users/1")
      .get("/api/v1/users/1")
      .check(status.is(200)))

  setUp(
    scenario.inject(
      rampUsers(100).during(30.seconds),    // ramp to 100 concurrent users over 30s
      constantUsersPerSec(50).during(60)    // hold 50 req/sec for 60s
    )
  ).protocols(httpProtocol)
    .assertions(
      global.responseTime.p99.lt(500),     // p99 < 500ms
      global.failedRequests.percent.lt(1)  // < 1% failures
    )
}
```

[↑ Back to top](#table-of-contents)

---

## Step 15 · Configuration & Secrets Management ⭐
> Managing config across environments safely — missing from most guides

### In this step

**💡 Concepts** — configuration patterns for multi-environment apps
- Externalise all config — 12-factor app ✅
- Profiles — environment-specific config ✅
- Never commit secrets ⭐
- Secret rotation ⭐

**🛠️ Tools** — configuration and secrets management
- application.yml / application.properties ✅
- Spring Profiles ✅
- Environment variables ✅
- Spring Cloud Config ⭐
- HashiCorp Vault ⭐
- AWS Secrets Manager ⭐
- Kubernetes Secrets ⭐

---

### 💡 Concepts

#### Externalise All Config ✅

```yaml
# application.yml — defaults for all environments
server:
  port: 8080

spring:
  application:
    name: order-service
  datasource:
    url: ${DB_URL}             # from environment variable
    username: ${DB_USERNAME}
    password: ${DB_PASSWORD}   # never hardcode

app:
  jwt:
    secret: ${JWT_SECRET}
    expiry: ${JWT_EXPIRY_SECONDS:3600}   # default to 3600 if not set
  payment:
    api-url: ${PAYMENT_API_URL}
    api-key: ${PAYMENT_API_KEY}
```

#### Spring Profiles ✅

```yaml
# application-dev.yml
spring:
  datasource:
    url: jdbc:h2:mem:devdb
logging:
  level:
    com.example: DEBUG

---
# application-prod.yml
spring:
  datasource:
    url: ${DB_URL}
logging:
  level:
    com.example: INFO
    root: WARN
```

```bash
# Activate profile
java -jar app.jar --spring.profiles.active=prod
# or
SPRING_PROFILES_ACTIVE=prod java -jar app.jar
```

#### Never Commit Secrets ⭐

```bash
# .gitignore
application-secrets.yml
.env
*.key
*.pem
```

Use environment variables, Vault, or a secrets manager. Never hardcode API keys, database passwords, or JWT secrets — even in non-production config files.

---

### 🛠️ Tools

#### HashiCorp Vault ⭐

Central secrets management — rotate secrets without redeployment.

```xml
<dependency>
  <groupId>org.springframework.cloud</groupId>
  <artifactId>spring-cloud-starter-vault-config</artifactId>
</dependency>
```

```yaml
spring:
  cloud:
    vault:
      uri: https://vault.example.com
      token: ${VAULT_TOKEN}
      kv:
        enabled: true
        default-context: order-service
```

#### Spring Cloud Config ⭐

Centralised configuration server — all services read config from one place.

```yaml
# Config server — serves config from Git
spring:
  cloud:
    config:
      server:
        git:
          uri: https://github.com/your-org/config-repo

# Config client — reads from config server
spring:
  config:
    import: configserver:http://config-server:8888
```

#### AWS Secrets Manager ⭐

```xml
<dependency>
  <groupId>io.awspring.cloud</groupId>
  <artifactId>spring-cloud-aws-secrets-manager</artifactId>
</dependency>
```

```yaml
spring:
  config:
    import: aws-secretsmanager:/myapp/prod/db-credentials
```

[↑ Back to top](#table-of-contents)

---

## Step 16 · Documentation & DX ⭐
> Keeping the codebase understandable and the team productive — missing from most guides

### In this step

**💡 Concepts** — practices that pay off as the team grows
- Architecture Decision Records (ADRs) ⭐
- API-first development ⭐
- Runbooks ⭐

**🛠️ Tools** — documentation and developer experience tooling
- OpenAPI / Swagger UI ✅
- Spring REST Docs ⭐
- Makefile / task runner ⭐
- Multi-module project structure ⭐
- Dev containers ⭐

---

### 💡 Concepts

#### Architecture Decision Records (ADRs) ⭐

Record significant technical decisions so future team members understand the why.

```
docs/adr/
  001-why-postgresql-over-mysql.md
  002-why-kafka-over-rabbitmq.md
  003-why-hexagonal-architecture.md
  004-why-flyway-over-liquibase.md
```

Format: **Context → Decision → Consequences → Alternatives considered**

#### API-First Development ⭐

Write the OpenAPI spec before writing any code. Frontend, mobile, and partner teams can generate clients and start work in parallel.

```yaml
# openapi.yml — write this first
openapi: "3.1.0"
info:
  title: Order Service API
  version: "1.0"
paths:
  /api/v1/orders:
    post:
      summary: Create order
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/CreateOrderRequest'
```

Then generate server stubs and client SDKs using **OpenAPI Generator**.

#### Runbooks ⭐

Document how to operate the service in production.

```
docs/runbooks/
  restart-service.md
  rollback-deployment.md
  investigate-high-error-rate.md
  database-connection-exhausted.md
  kafka-consumer-lag-growing.md
```

---

### 🛠️ Tools

#### OpenAPI / Swagger UI ✅

Auto-generated interactive API documentation.

```xml
<dependency>
  <groupId>org.springdoc</groupId>
  <artifactId>springdoc-openapi-starter-webmvc-ui</artifactId>
  <version>2.5.0</version>
</dependency>
```

Accessible at: `http://localhost:8080/swagger-ui.html`

#### Spring REST Docs ⭐

Generates API documentation from tests — documentation is always accurate because it comes from passing tests.

```java
@Test
void createUser_isDocumented() throws Exception {
  mockMvc.perform(post("/api/v1/users")
      .contentType(APPLICATION_JSON)
      .content("""{"email":"alice@example.com","password":"secret123"}"""))
    .andExpect(status().isCreated())
    .andDo(document("create-user",
      requestFields(
        fieldWithPath("email").description("User's email address"),
        fieldWithPath("password").description("Password (min 8 chars)")
      ),
      responseFields(
        fieldWithPath("id").description("Generated user ID"),
        fieldWithPath("email").description("User's email address")
      )
    ));
}
```

#### Dev Containers ⭐

One command to get a complete local development environment — database, Kafka, Redis, all running.

```json
// .devcontainer/devcontainer.json
{
  "name": "Order Service",
  "dockerComposeFile": "../docker-compose.yml",
  "service": "app",
  "postCreateCommand": "sdk install java 21 && ./gradlew build"
}
```

[↑ Back to top](#table-of-contents)

---

## Quick Reference — Full Recommended Stack

```
Foundation           →  Spring Boot 3.x + Java 21 + Gradle (Kotlin DSL)
Language             →  Java (Records, Sealed classes, Virtual threads) or Kotlin ⭐
Boilerplate          →  Lombok ✅ or Java Records ⭐
Code quality         →  Checkstyle + SpotBugs ⭐ + SonarQube ⭐ + Spotless ⭐
Architecture         →  Package by feature ⭐ or Hexagonal ⭐ for complex domains
Core pattern         →  Controller → Service → Repository ✅
Data transfer        →  DTO pattern ✅ + MapStruct ✅
Design patterns      →  Strategy ⭐, Factory ⭐, Decorator ⭐ where applicable
ORM                  →  Spring Data JPA + Hibernate ✅
Dynamic queries      →  QueryDSL ⭐ or JOOQ ⭐
Database             →  PostgreSQL ✅
Migrations           →  Flyway ✅
Data patterns        →  Pagination ⭐, Projections ⭐, Fetch joins ⭐, Optimistic locking ⭐
API style            →  REST (Spring MVC) ✅ or GraphQL ⭐ for flexible queries
API docs             →  OpenAPI / Swagger UI ✅ + Spring REST Docs ⭐
API versioning       →  URI versioning (/api/v1/) ⭐
Error handling       →  @ControllerAdvice + standard ApiError format ⭐
Validation           →  Jakarta Validation + custom validators ⭐
Security             →  Spring Security ✅ + OAuth2 Resource Server ⭐ + Keycloak ⭐
Password hashing     →  BCrypt ✅
Messaging            →  Apache Kafka ✅ + Outbox pattern ⭐
In-process events    →  Spring Application Events ✅
Caching              →  Spring Cache ✅ + Redis ✅ + Caffeine (L1) ✅
Resilience           →  Resilience4j ✅ (Circuit breaker + Retry + Timeout) ⭐
Retry                →  Spring Retry ✅
Logs                 →  Structured JSON logging ⭐ + Correlation IDs ⭐
Metrics              →  Micrometer + Prometheus + Grafana ✅
Tracing              →  OpenTelemetry + Jaeger / Zipkin ⭐
Health checks        →  Spring Boot Actuator ✅ + liveness/readiness probes ⭐
Unit testing         →  JUnit 5 + Mockito + AssertJ ✅
Integration testing  →  @WebMvcTest + @DataJpaTest + Testcontainers ⭐
API mocking          →  WireMock ⭐
Load testing         →  Gatling ⭐
Containerisation     →  Docker + Docker Compose ✅
Orchestration        →  Kubernetes + Helm ⭐
CI/CD                →  GitHub Actions ✅
Cloud                →  AWS (ECS/EKS) ✅
Config management    →  Spring Profiles ✅ + environment variables ✅
Secrets management   →  HashiCorp Vault ⭐ or AWS Secrets Manager ⭐
Documentation        →  OpenAPI ✅ + ADRs ⭐ + Runbooks ⭐
```

> ⭐ = Added — commonly missing from standard Spring Boot guides
> The ⭐ additions (resilience, observability, messaging, caching, hexagonal architecture, secrets management)
> are the difference between a Spring Boot app that works in demos and one that runs reliably in production.

---

*Each layer depends on the one before it. Build the foundation right and every subsequent layer is easier to add.*