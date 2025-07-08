# 🍺 House of 30mL - Order Service API Documentation

## 📋 **Project Overview**

**Service Name:** Order Service  
**Version:** 6.0.0  
**Base URL:** `http://localhost:8080`  
**Framework:** Spring Boot 3.3.2  
**Java Version:** 17  
**Database:** MySQL  

---

## 🏗️ **Project Architecture**

```
📁 Project Structure
├── 🎮 Controllers (API Layer)
├── 🧠 Services (Business Logic)
├── 💾 Data Layer (Database Access)
├── 📝 Models (Request/Response DTOs)
├── ⚙️ Configuration
└── 🛠️ Utilities & Helpers
```

### **Key Components:**
- **REST Controllers:** Handle HTTP requests/responses
- **Service Layer:** Business logic processing
- **Data Access Layer:** Database operations via JPA
- **Security:** JWT tokens for QR authentication
- **Payment Integration:** Razorpay payment gateway
- **File Storage:** AWS S3 for invoices

---

## 🔐 **Authentication & Security**

### **API Key Authentication**
All requests require an API key in headers:
```http
ApiKey: 30ML-LcV9mdJvKhbupWn1xOGexe4mByO
Content-Type: application/json
```

### **JWT Tokens**
Used for QR code authentication:
- **Reservation QR:** JWT with reservation details
- **Order QR:** JWT with order details
- **Expiration:** Configurable timeout

---

## 📊 **Standard Response Format**

All APIs return responses in this format:

```json
{
  "status": {
    "statusCode": 200,
    "statusMessage": "Success message"
  },
  "data": {
    // Response data here
  }
}
```

### **HTTP Status Codes:**
- `200` - Success
- `400` - Bad Request (validation error)
- `401` - Unauthorized
- `404` - Not Found
- `500` - Internal Server Error

---

## 🎯 **API Endpoints**

## 1. **Application Management**

### **Get Application Version**
```http
GET /application/version
```

**Response:**
```json
{
  "data": {
    "version": "6.0.0"
  },
  "status": {
    "statusCode": 200,
    "statusMessage": "Sending The version of the app"
  }
}
```

---

## 2. **Cart Management** 🛒

### **Get Cart Items**
```http
POST /cart/test/getCart
```

**Request Body:**
```json
{
  "userId": "123",
  "pubId": "456"
}
```

**Response:**
```json
{
  "status": {
    "statusCode": 200,
    "statusMessage": "Cart retrieved successfully"
  },
  "items": [
    {
      "drinkId": "789",
      "name": "Beer",
      "quantityAdded": 2,
      "appListingPrice": 300.0,
      "pubListingPrice": 350.0,
      "commPrice": 280.0,
      "isAvailable": true,
      "drinkCategpry": 1,
      "image": {
        "imageUrl": "https://example.com/beer.jpg"
      }
    }
  ]
}
```

### **Add/Update Cart Item**
```http
POST /cart/test/addOrUpdateCart
```

**Request Body:**
```json
{
  "userId": "123",
  "drinkId": "789",
  "drinkQuantity": "2",
  "pubId": "456",
  "isCombo": false
}
```

---

## 3. **Order Management** 📋

### **Get My Orders**
```http
POST /orders/test/myOrders
```

**Request Body:**
```json
{
  "userId": "123"
}
```

**Response:**
```json
{
  "status": {
    "statusCode": 200,
    "statusMessage": "Orders retrieved successfully"
  },
  "items": [
    {
      "orderId": "1001",
      "orderItemId": "2001",
      "drinkId": "789",
      "name": "Beer",
      "orderDateTime": "2024-01-15 14:30:00",
      "quantity": 2,
      "price": 600.0,
      "paymentStatus": "PAID",
      "media": {
        "imageUrl": "https://example.com/beer.jpg"
      }
    }
  ]
}
```

### **Get Orders List for Pub (Owner)**
```http
POST /orders/test/getOrdersListForPub
```

**Request Body:**
```json
{
  "pubId": "456",
  "orderPlaceDate": "2024-01-15",
  "orderType": "DINE_IN"
}
```

### **Get Order Items for Specific Order**
```http
POST /orders/test/getOrderItemsForOrder
```

**Request Body:**
```json
{
  "redeemId": "1001"
}
```

### **Change Order Status (Owner)**
```http
POST /orders/test/ownerChangeStatusOfOrder
```

**Request Body:**
```json
{
  "orderId": "1001",
  "orderStatus": "PREPARING"
}
```

**Order Status Values:**
- `PENDING`
- `CONFIRMED`
- `PREPARING`
- `READY`
- `COMPLETED`
- `CANCELLED`

---

## 4. **Payment Management** 💳

### **Initiate Payment**
```http
POST /payment/test/initiate-payment
```

**Request Body:**
```json
{
  "userId": "123",
  "amount": "600",
  "currency": "INR"
}
```

**Response:**
```json
{
  "status": {
    "statusCode": 200,
    "statusMessage": "Payment initiated successfully"
  },
  "orderId": "order_1234567890",
  "amount": 600,
  "currency": "INR",
  "razorpayOrderId": "rzp_order_1234567890"
}
```

### **Register Payment Callback**
```http
POST /payment/test/register-callback
```

**Request Body:**
```json
{
  "razorpay_payment_id": "pay_1234567890",
  "razorpay_order_id": "order_1234567890",
  "razorpay_signature": "signature_hash"
}
```

### **Accept Webhook**
```http
POST /payment/test/accept-webhook
```

**Request Body:**
```json
{
  "payload": {
    "payment": {
      "entity": {
        "id": "pay_1234567890",
        "status": "captured"
      }
    }
  }
}
```

---

## 5. **Wallet Management** 💰

### **Initiate Wallet Payment**
```http
POST /wallet/initiate-payment
```

**Request Body:**
```json
{
  "userId": "123",
  "amount": "500",
  "currency": "INR"
}
```

### **Get Wallet Payment for User**
```http
POST /wallet/getWalletPaymentForUserId
```

**Request Body:**
```json
{
  "userId": "123"
}
```

---

## 6. **Reservation Management** 🪑

### **Create New Reservation**
```http
POST /reservation/newReservation
```

**Request Body:**
```json
{
  "userId": "123",
  "pubId": "456",
  "startDate": "15/01/2024",
  "reservationTime": "19:30",
  "invitedPhoneNumbers": ["+919876543210", "+919876543211"],
  "totalMale": 2,
  "totalFemale": 1,
  "totalAmount": 1000
}
```

### **Get My Reservations**
```http
POST /reservation/myReservations/{direction}
```

**Path Parameters:**
- `direction`: `present` or `past`

**Request Body:**
```json
{
  "userId": "123"
}
```

### **Fetch Reservation Details**
```http
POST /reservation/fetchReservation
```

**Request Body:**
```json
{
  "reservationId": "789"
}
```

### **Create New Invite**
```http
POST /reservation/newInvite
```

**Request Body:**
```json
{
  "reservationId": "789",
  "phoneNumbers": ["+919876543212"]
}
```

### **Get Reserved Users for Date (Pub Owner)**
```http
POST /reservation/getReservedUsersForADate
```

**Request Body:**
```json
{
  "pubId": "456",
  "reservationDate": "2024-01-15",
  "checkedInStatus": "CHECKED_IN",
  "reachedPub": true
}
```

---

## 7. **QR Code Management** 🎫

### **Generate Reservation QR**
```http
POST /qr/encodeReservation
```

**Request Body:**
```json
{
  "reservationId": "789",
  "userId": "123",
  "generationTime": "2024-01-15T14:30:00"
}
```

### **Generate Order QR**
```http
POST /qr/encodeRedeem
```

**Request Body:**
```json
{
  "orderId": "1001",
  "orderStatus": "READY",
  "generationTime": "2024-01-15T14:30:00"
}
```

**Response:**
```json
{
  "status": {
    "statusCode": 200,
    "statusMessage": "QR generated successfully"
  },
  "encodedString": "eyJhbGciOiJIUzI1NiJ9.eyJvcmRlcklkIjoiMTAwMSIsIm9yZGVyU3RhdHVzIjoiUkVBRFkifQ.signature"
}
```

---

## 8. **Drink Redemption** 🍻

### **Get My Redeems**
```http
POST /redeem/myRedeems
```

**Request Body:**
```json
{
  "userId": "123",
  "status": "PENDING",
  "types": "current"
}
```

**Redeem Status Values:**
- `PENDING`
- `COMPLETED`
- `CANCELLED`
- `all`

---

## 🔧 **Error Handling**

### **Validation Errors**
```json
{
  "status": {
    "statusCode": 400,
    "statusMessage": "Validation failed: userId is required"
  }
}
```

### **Server Errors**
```json
{
  "status": {
    "statusCode": 500,
    "statusMessage": "Internal server error occurred"
  }
}
```

---

## 📝 **Request/Response Models**

### **Common Models**

#### **Status**
```json
{
  "statusCode": 200,
  "statusMessage": "Success message"
}
```

#### **MediaResp**
```json
{
  "imageUrl": "https://example.com/image.jpg"
}
```

---

## 🚀 **Getting Started**

### **Prerequisites**
- Java 17
- MySQL Database
- Maven 3.6+
- Razorpay Account (for payments)
- AWS Account (for S3 storage)

### **Environment Variables**
```bash
DB_API_URL=https://30mlapi.azurewebsites.net/api/
DB_API_KEY=your-api-key
JWT_ORDER_SECRET_KEY=your-jwt-secret
RZP_KEY_ID=your-razorpay-key
RZP_KEY_SECRET=your-razorpay-secret
```

### **Running the Application**
```bash
mvn spring-boot:run
```

**Health Check:**
```http
GET /actuator/health
```

---

## 📋 **Complete API Endpoint Reference**

### **Application Management**
| Method | Endpoint | Description |
|--------|----------|-------------|
| `GET` | `/application/version` | Get application version |
| `GET` | `/actuator/health` | Health check endpoint |

### **Cart Management**
| Method | Endpoint | Description |
|--------|----------|-------------|
| `POST` | `/cart/test/getCart` | Retrieve user's cart items |
| `POST` | `/cart/test/addOrUpdateCart` | Add or update cart item |

### **Order Management**
| Method | Endpoint | Description |
|--------|----------|-------------|
| `POST` | `/orders/test/myOrders` | Get user's order history |
| `POST` | `/orders/test/getOrdersListForPub` | Get orders for pub (owner) |
| `POST` | `/orders/test/getOrderItemsForOrder` | Get items for specific order |
| `POST` | `/orders/test/ownerChangeStatusOfOrder` | Change order status (owner) |

### **Payment Management**
| Method | Endpoint | Description |
|--------|----------|-------------|
| `POST` | `/payment/test/initiate-payment` | Start payment process |
| `POST` | `/payment/test/register-callback` | Handle payment callback |
| `POST` | `/payment/test/accept-webhook` | Accept payment webhook |
| `POST` | `/payment/test/transaction-webhook` | Transaction webhook handler |

### **Wallet Management**
| Method | Endpoint | Description |
|--------|----------|-------------|
| `POST` | `/wallet/initiate-payment` | Initiate wallet payment |
| `POST` | `/wallet/accept-webhook` | Accept wallet webhook |
| `POST` | `/wallet/getWalletPaymentForUserId` | Get wallet payments |

### **Reservation Management**
| Method | Endpoint | Description |
|--------|----------|-------------|
| `POST` | `/reservation/newReservation` | Create new reservation |
| `POST` | `/reservation/myReservations/{direction}` | Get user reservations |
| `POST` | `/reservation/fetchReservation` | Get reservation details |
| `POST` | `/reservation/newInvite` | Create reservation invite |
| `POST` | `/reservation/getReservedUsersForADate` | Get reserved users (owner) |

### **QR Code Management**
| Method | Endpoint | Description |
|--------|----------|-------------|
| `POST` | `/qr/encodeReservation` | Generate reservation QR |
| `POST` | `/qr/encodeRedeem` | Generate order QR |

### **Drink Redemption**
| Method | Endpoint | Description |
|--------|----------|-------------|
| `POST` | `/redeem/myRedeems` | Get user's redemption history |

### **Special Features**
| Method | Endpoint | Description |
|--------|----------|-------------|
| `GET` | `/CheckExclusiveDay/test` | Check if today is exclusive day |

---

## 🔧 **Development Setup**

### **Local Development**
```bash
# Clone repository
git clone <repository-url>
cd orderService-main

# Set environment variables
export DB_API_URL=https://30mlapi.azurewebsites.net/api/
export DB_API_KEY=your-api-key
export JWT_ORDER_SECRET_KEY=your-jwt-secret

# Run application
mvn spring-boot:run
```

### **Docker Deployment**
```bash
# Build image
docker build -t house-of-30ml-order-service .

# Run container
docker run -p 8080:8080 \
  -e DB_API_URL=your-db-url \
  -e DB_API_KEY=your-api-key \
  house-of-30ml-order-service
```

### **Testing APIs**
```bash
# Health check
curl http://localhost:8080/actuator/health

# Get version
curl http://localhost:8080/application/version

# Test cart endpoint
curl -X POST http://localhost:8080/cart/test/getCart \
  -H "Content-Type: application/json" \
  -H "ApiKey: 30ML-LcV9mdJvKhbupWn1xOGexe4mByO" \
  -d '{"userId":"123","pubId":"456"}'
```

---

## 📞 **Support**

For API support and documentation updates, contact the development team.

**Base URL:** `http://localhost:8080`
**Health Endpoint:** `/actuator/health`
**Version:** 6.0.0
**Documentation:** See `Project_Structure_Guide.md` for detailed architecture
