# AI Usage Documentation

## PeerIslands E-commerce Order Processing System

### 🤖 Overview
This document details how AI assistance (primarily ChatGPT/Perplexity Comet) was utilized during the development of this Spring Boot backend application for the PeerIslands coding assignment.

---

### 📝 AI Tools Used
1. **Perplexity Comet** - Primary AI assistant
2. **Browser Automation** - For GitHub repository setup and file management

---

### ✨ How AI Was Used

#### 1. **Project Structure & Setup**
**What AI Helped With:**
- Generating the complete Maven project structure
- Creating pom.xml with appropriate Spring Boot dependencies
- Setting up proper package structure following Spring Boot best practices

**Example:**
```xml
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
    <!-- Other dependencies -->
</dependencies>
```

**Verification:**
- Reviewed all dependencies for version compatibility
- Ensured H2 database was configured as in-memory
- Validated Maven build configuration

---

#### 2. **Entity & Model Classes**
**What AI Helped With:**
- Designing the `Order` and `OrderItem` entity relationships
- Implementing JPA annotations correctly
- Creating the `OrderStatus` enum with proper values

**Issues Found:**
- Initial design had bi-directional relationship without proper cascade settings
- **Solution:** Added `CascadeType.ALL` and `orphanRemoval=true` for proper entity management

**Example:**
```java
@OneToMany(mappedBy = "order", cascade = CascadeType.ALL, orphanRemoval = true)
private List<OrderItem> items = new ArrayList<>();
```

---

#### 3. **Repository Layer**
**What AI Helped With:**
- Creating JpaRepository interface with custom query methods
- Implementing status-based filtering

**Code Generated:**
```java
public interface OrderRepository extends JpaRepository<Order, Long> {
    List<Order> findByStatus(OrderStatus status);
    List<Order> findByCustomerEmail(String customerEmail);
}
```

**Verification:**
- Tested query method naming conventions
- Ensured Spring Data JPA could auto-implement these methods

---

#### 4. **Service Layer with Scheduled Jobs**
**What AI Helped With:**
- Implementing the business logic for order operations
- Creating the @Scheduled background job for auto-updating PENDING orders
- Adding proper transaction management with @Transactional

**Issues Found:**
- Initial scheduled job used incorrect cron expression
- **Solution:** Changed to `fixedRate = 300000` (5 minutes in milliseconds)

**Code:**
```java
@Scheduled(fixedRate = 300000) // 5 minutes
public void processPendingOrders() {
    List<Order> pendingOrders = orderRepository.findByStatus(OrderStatus.PENDING);
    pendingOrders.forEach(order -> {
        order.setStatus(OrderStatus.PROCESSING);
        orderRepository.save(order);
    });
    log.info("Processed {} pending orders", pendingOrders.size());
}
```

**Correction Made:**
- Added proper logging
- Ensured @EnableScheduling was added to main application class

---

#### 5. **REST Controller & API Endpoints**
**What AI Helped With:**
- Designing RESTful endpoints following best practices
- Implementing proper HTTP status codes
- Creating request/response DTOs

**Endpoints Created:**
- POST `/api/orders` - Create order
- GET `/api/orders` - List all orders (with optional status filter)
- GET `/api/orders/{id}` - Get order by ID
- PUT `/api/orders/{id}/status` - Update order status
- DELETE `/api/orders/{id}` - Cancel order

**Issues Found:**
- Initial design exposed entity classes directly in responses
- **Solution:** Created separate DTO classes (OrderRequest, OrderResponse) for better API design

---

#### 6. **Exception Handling**
**What AI Helped With:**
- Implementing global exception handler with @ControllerAdvice
- Creating custom exceptions (OrderNotFoundException)
- Proper error response formatting

**Example:**
```java
@ExceptionHandler(OrderNotFoundException.class)
public ResponseEntity<ErrorResponse> handleOrderNotFound(OrderNotFoundException ex) {
    return ResponseEntity.status(HttpStatus.NOT_FOUND)
        .body(new ErrorResponse(ex.getMessage()));
}
```

---

#### 7. **Configuration Files**
**What AI Helped With:**
- Creating application.properties with H2 configuration
- Setting up H2 console access
- Configuring JPA and Hibernate properties

**Configuration:**
```properties
spring.h2.console.enabled=true
spring.datasource.url=jdbc:h2:mem:ecommerce
spring.jpa.hibernate.ddl-auto=update
```

---

#### 8. **Documentation**
**What AI Helped With:**
- Creating comprehensive README.md with:
  - Project overview
  - Setup instructions
  - API documentation
  - cURL examples
- Formatting with proper Markdown
- Adding emojis for better readability

---

### ⚠️ Issues Found & Corrected

| Issue | AI-Generated Solution | Manual Correction |
|-------|----------------------|-------------------|
| Order cancellation allowed for any status | Only check if status == PENDING | Added business logic validation |
| No total amount calculation | Basic calculation | Added helper method with proper decimal handling |
| Missing validation annotations | Added @NotNull, @NotBlank | Added @Email, @Min validations |
| Scheduled job timing | Used cron expression | Changed to fixedRate for simplicity |
| Direct entity exposure | Used entities in API | Created DTO layer |

---

### ✅ Testing & Validation

**AI-Assisted Testing:**
1. Generated sample cURL commands for API testing
2. Created test data scenarios
3. Validated H2 console queries

**Manual Verification:**
1. Tested all API endpoints with Postman
2. Verified scheduled job execution
3. Checked database schema in H2 console
4. Validated business logic constraints

---

### 📊 AI Contribution Summary

**Percentage Breakdown:**
- Project Structure & Boilerplate: ~80% AI
- Business Logic Implementation: ~60% AI
- Error Handling: ~70% AI
- Documentation: ~85% AI
- Testing & Refinement: ~30% AI, 70% Manual

**Overall: ~65% AI-assisted, 35% manual development and refinement**

---

### 📚 Learnings

**What Worked Well:**
1. AI quickly generated boilerplate code and project structure
2. Saved significant time on documentation and README creation
3. Provided good starting points for complex implementations

**What Required Manual Intervention:**
1. Business logic edge cases
2. Proper error handling for specific scenarios
3. Performance optimization considerations
4. Security best practices

**Best Practices Followed:**
1. Always reviewed and understood AI-generated code
2. Tested each component independently
3. Refactored when necessary for better design
4. Added comments for complex logic

---

### 🔧 Tools & Technologies

**Development:**
- IntelliJ IDEA / VS Code
- Maven
- Postman (API testing)
- H2 Console

**AI-Assisted:**
- Code generation
- Documentation
- Problem-solving
- Best practices guidance

---

### 🎯 Conclusion

AI assistance significantly accelerated the development process, especially for:
- Repetitive boilerplate code
- Documentation
- Initial implementation patterns
- Problem identification

However, human oversight was crucial for:
- Business logic correctness
- Security considerations
- Performance optimization
- Real-world testing

This project demonstrates effective human-AI collaboration where AI handles routine tasks while human developers focus on critical thinking, validation, and refinement.

---

**Developer**: Vishal Khatri  
**Date**: November 8, 2025  
**Assignment**: PeerIslands Backend Developer Coding Assessment
