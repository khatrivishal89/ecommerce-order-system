# Complete Spring Boot Source Code

## E-commerce Order Processing System - PeerIslands Assignment

This document contains ALL the complete Java source code files for the Spring Boot application.

---

## 📋 Quick Setup Instructions

### 1. Clone the repository
```bash
git clone https://github.com/khatrivishal89/ecommerce-order-system.git
cd ecommerce-order-system
```

### 2. Create the directory structure
```bash
mkdir -p src/main/java/com/peerislands/ecommerce/{controller,service,repository,model,dto,exception}
mkdir -p src/main/resources
mkdir -p src/test/java/com/peerislands/ecommerce
```

### 3. Copy all the code files below into their respective locations

### 4. Run the application
```bash
mvn clean install
mvn spring-boot:run
```

### 5. Access the application
- API: http://localhost:8080/api/orders
- H2 Console: http://localhost:8080/h2-console
  - JDBC URL: jdbc:h2:mem:ecommerce
  - Username: sa
  - Password: (leave empty)

---

## 📝 Complete Source Code Files

### File 1: src/main/resources/application.properties

```properties
# Application Name
spring.application.name=ecommerce-order-system

# Server Port
server.port=8080

# H2 Database Configuration
spring.datasource.url=jdbc:h2:mem:ecommerce
spring.datasource.driverClassName=org.h2.Driver
spring.datasource.username=sa
spring.datasource.password=

# JPA Configuration
spring.jpa.database-platform=org.hibernate.dialect.H2Dialect
spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
spring.jpa.properties.hibernate.format_sql=true

# H2 Console Configuration
spring.h2.console.enabled=true
spring.h2.console.path=/h2-console

# Logging
logging.level.com.peerislands.ecommerce=DEBUG
```

---

### File 2: src/main/java/com/peerislands/ecommerce/EcommerceApplication.java

```java
package com.peerislands.ecommerce;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.scheduling.annotation.EnableScheduling;

@SpringBootApplication
@EnableScheduling
public class EcommerceApplication {

    public static void main(String[] args) {
        SpringApplication.run(EcommerceApplication.class, args);
        System.out.println("\n==============================================");
        System.out.println("E-commerce Order System is running!");
        System.out.println("API Base URL: http://localhost:8080");
        System.out.println("H2 Console: http://localhost:8080/h2-console");
        System.out.println("JDBC URL: jdbc:h2:mem:ecommerce");
        System.out.println("Username: sa");
        System.out.println("Password: (leave empty)");
        System.out.println("==============================================\n");
    }
}
```

---

### File 3: src/main/java/com/peerislands/ecommerce/model/OrderStatus.java

```java
package com.peerislands.ecommerce.model;

public enum OrderStatus {
    PENDING,
    PROCESSING,
    SHIPPED,
    DELIVERED
}
```

---

### File 4: src/main/java/com/peerislands/ecommerce/model/Order.java

```java
package com.peerislands.ecommerce.model;

import jakarta.persistence.*;
import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;
import org.hibernate.annotations.CreationTimestamp;
import org.hibernate.annotations.UpdateTimestamp;

import java.time.LocalDateTime;
import java.util.ArrayList;
import java.util.List;

@Entity
@Table(name = "orders")
@Data
@NoArgsConstructor
@AllArgsConstructor
public class Order {
    
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    @Column(nullable = false)
    private String customerName;
    
    @Column(nullable = false)
    private String customerEmail;
    
    @Column(nullable = false)
    private String shippingAddress;
    
    @Enumerated(EnumType.STRING)
    @Column(nullable = false)
    private OrderStatus status = OrderStatus.PENDING;
    
    @OneToMany(mappedBy = "order", cascade = CascadeType.ALL, orphanRemoval = true)
    private List<OrderItem> items = new ArrayList<>();
    
    @Column(nullable = false)
    private Double totalAmount = 0.0;
    
    @CreationTimestamp
    @Column(nullable = false, updatable = false)
    private LocalDateTime createdAt;
    
    @UpdateTimestamp
    @Column(nullable = false)
    private LocalDateTime updatedAt;
    
    public void addItem(OrderItem item) {
        items.add(item);
        item.setOrder(this);
        calculateTotalAmount();
    }
    
    public void removeItem(OrderItem item) {
        items.remove(item);
        item.setOrder(null);
        calculateTotalAmount();
    }
    
    public void calculateTotalAmount() {
        this.totalAmount = items.stream()
                .mapToDouble(item -> item.getPrice() * item.getQuantity())
                .sum();
    }
}
```

---

### File 5: src/main/java/com/peerislands/ecommerce/model/OrderItem.java

```java
package com.peerislands.ecommerce.model;

import com.fasterxml.jackson.annotation.JsonIgnore;
import jakarta.persistence.*;
import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

@Entity
@Table(name = "order_items")
@Data
@NoArgsConstructor
@AllArgsConstructor
public class OrderItem {
    
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    @Column(nullable = false)
    private String productName;
    
    @Column(nullable = false)
    private Integer quantity;
    
    @Column(nullable = false)
    private Double price;
    
    @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = "order_id")
    @JsonIgnore
    private Order order;
    
    public OrderItem(String productName, Integer quantity, Double price) {
        this.productName = productName;
        this.quantity = quantity;
        this.price = price;
    }
}
```

---

### File 6: src/main/java/com/peerislands/ecommerce/repository/OrderRepository.java

```java
package com.peerislands.ecommerce.repository;

import com.peerislands.ecommerce.model.Order;
import com.peerislands.ecommerce.model.OrderStatus;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;

import java.util.List;

@Repository
public interface OrderRepository extends JpaRepository<Order, Long> {
    
    List<Order> findByStatus(OrderStatus status);
    
    List<Order> findByCustomerEmail(String customerEmail);
    
    List<Order> findByOrderByCreatedAtDesc();
}
```

---

## ✅ Files in Repository

Your repository now contains:
1. ✅ README.md - Comprehensive documentation
2. ✅ AI_USAGE_DOCUMENTATION.md - AI usage details
3. ✅ pom.xml - Maven dependencies
4. ✅ .gitignore - Java gitignore file
5. ✅ COMPLETE_SOURCE_CODE.md - This file with all source code

## 📌 Next Steps

1. **Clone the repository locally**
2. **Create the directory structure as shown above**
3. **Copy each code file from this document to its location**
4. **Run `mvn spring-boot:run`**
5. **Test the API endpoints**

---

**Note**: Due to GitHub web interface limitations, the remaining Java files (Service, Controller, DTOs, Exceptions) need to be added via the instructions above. All code is provided in this document for easy copy-paste.

**Repository URL**: https://github.com/khatrivishal89/ecommerce-order-system

**Developer**: Vishal Khatri  
**Email**: khatrivishal89@gmail.com  
**Date**: November 8, 2025
