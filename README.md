# E-commerce Order Processing System

## PeerIslands Backend Developer Assignment

### 📋 Project Overview
A RESTful backend system built with Spring Boot for managing e-commerce orders. This system handles order creation, status tracking, automatic order processing, and order cancellation.

### ✨ Features
- ✅ Create orders with multiple items
- ✅ Retrieve order details by ID
- ✅ List all orders with optional status filtering
- ✅ Update order status (PENDING → PROCESSING → SHIPPED → DELIVERED)
- ✅ Automatic background job: Updates PENDING orders to PROCESSING every 5 minutes
- ✅ Cancel orders (only PENDING orders can be cancelled)
- ✅ H2 in-memory database
- ✅ RESTful API design
- ✅ Comprehensive error handling

### 🛠️ Technology Stack
- **Java**: 17
- **Spring Boot**: 3.2.0
- **Spring Data JPA**: For database operations
- **H2 Database**: In-memory database
- **Maven**: Build tool
- **Lombok**: Reduce boilerplate code

### 📁 Project Structure
```
ecommerce-order-system/
├── src/
│   ├── main/
│   │   ├── java/com/peerislands/ecommerce/
│   │   │   ├── EcommerceApplication.java
│   │   │   ├── controller/
│   │   │   │   └── OrderController.java
│   │   │   ├── service/
│   │   │   │   └── OrderService.java
│   │   │   ├── repository/
│   │   │   │   └── OrderRepository.java
│   │   │   ├── model/
│   │   │   │   ├── Order.java
│   │   │   │   ├── OrderItem.java
│   │   │   │   └── OrderStatus.java
│   │   │   ├── dto/
│   │   │   │   ├── OrderRequest.java
│   │   │   │   └── OrderResponse.java
│   │   │   └── exception/
│   │   │       ├── OrderNotFoundException.java
│   │   │       └── GlobalExceptionHandler.java
│   │   └── resources/
│   │       └── application.properties
│   └── test/
│       └── java/com/peerislands/ecommerce/
├── pom.xml
├── README.md
├── AI_USAGE_DOCUMENTATION.md
└── .gitignore
```

### 🚀 Quick Start Guide

#### Prerequisites
- Java 17 or higher
- Maven 3.6+
- Git

#### Installation Steps

1. **Clone the repository**
   ```bash
   git clone https://github.com/khatrivishal89/ecommerce-order-system.git
   cd ecommerce-order-system
   ```

2. **Build the project**
   ```bash
   mvn clean install
   ```

3. **Run the application**
   ```bash
   mvn spring-boot:run
   ```

4. **Access the application**
   - API Base URL: `http://localhost:8080`
   - H2 Console: `http://localhost:8080/h2-console`
     - JDBC URL: `jdbc:h2:mem:ecommerce`
     - Username: `sa`
     - Password: (leave empty)

### 📡 API Endpoints

#### 1. Create Order
```http
POST /api/orders
Content-Type: application/json

{
  "customerName": "John Doe",
  "customerEmail": "john@example.com",
  "shippingAddress": "123 Main St, City, Country",
  "items": [
    {
      "productName": "Laptop",
      "quantity": 1,
      "price": 1200.00
    },
    {
      "productName": "Mouse",
      "quantity": 2,
      "price": 25.00
    }
  ]
}
```

#### 2. Get Order by ID
```http
GET /api/orders/{id}
```

#### 3. Get All Orders
```http
GET /api/orders

# With status filter
GET /api/orders?status=PENDING
```

#### 4. Update Order Status
```http
PUT /api/orders/{id}/status?status=PROCESSING
```

#### 5. Cancel Order
```http
DELETE /api/orders/{id}
```

### 🧪 Testing with cURL

```bash
# Create an order
curl -X POST http://localhost:8080/api/orders \
  -H "Content-Type: application/json" \
  -d '{
    "customerName": "John Doe",
    "customerEmail": "john@example.com",
    "shippingAddress": "123 Main St",
    "items": [
      {"productName": "Laptop", "quantity": 1, "price": 1200.00}
    ]
  }'

# Get all orders
curl http://localhost:8080/api/orders

# Get order by ID
curl http://localhost:8080/api/orders/1

# Update order status
curl -X PUT "http://localhost:8080/api/orders/1/status?status=PROCESSING"

# Cancel order
curl -X DELETE http://localhost:8080/api/orders/1
```

### 🔄 Background Job
The system includes a scheduled job that runs every 5 minutes to automatically update all PENDING orders to PROCESSING status.

### 📊 Database Schema

**Orders Table**
- id (Primary Key)
- customer_name
- customer_email
- shipping_address
- status (PENDING/PROCESSING/SHIPPED/DELIVERED)
- total_amount
- created_at
- updated_at

**Order Items Table**
- id (Primary Key)
- order_id (Foreign Key)
- product_name
- quantity
- price

