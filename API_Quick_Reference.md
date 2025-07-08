# 🚀 House of 30mL - API Quick Reference

## 📋 **What is an API?**

**API (Application Programming Interface)** is like a **waiter in a restaurant**:
- 📱 **You (Mobile App)** place an order
- 👨‍🍳 **Kitchen (Database)** prepares the food  
- 🍽️ **Waiter (API)** takes your order and brings back the result

---

## 🎯 **This Project's APIs**

Your **House of 30mL Order Service** provides APIs for a **pub ordering system** where customers can:
- 🛒 Browse drinks and manage cart
- 📋 Place and track orders
- 💳 Make payments via Razorpay
- 🪑 Reserve tables at pubs
- 🎫 Get QR codes for pickup/dining
- 📄 Receive digital invoices

---

## 🔧 **How APIs Work Here**

### **Request-Response Pattern:**
```
📱 Mobile App  →  🌐 API Request  →  🧠 Business Logic  →  💾 Database
📱 Mobile App  ←  📊 JSON Response  ←  🔄 Processing  ←  💾 Database
```

### **Example API Call:**
```http
POST /cart/test/getCart
Content-Type: application/json
ApiKey: 30ML-LcV9mdJvKhbupWn1xOGexe4mByO

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
    "statusMessage": "Success"
  },
  "items": [
    {
      "drinkId": "789",
      "name": "Beer",
      "quantityAdded": 2,
      "appListingPrice": 300.0
    }
  ]
}
```

---

## 📚 **API Categories**

### 1. **🛒 Cart APIs** - Shopping Cart Management
- **Get Cart:** View items in user's cart
- **Add/Update Cart:** Modify cart contents

### 2. **📋 Order APIs** - Order Processing
- **My Orders:** View order history
- **Pub Orders:** Orders for pub owners
- **Order Status:** Update order progress

### 3. **💳 Payment APIs** - Payment Processing
- **Initiate Payment:** Start Razorpay payment
- **Payment Callback:** Handle payment results
- **Webhooks:** Receive payment notifications

### 4. **🪑 Reservation APIs** - Table Booking
- **New Reservation:** Book a table
- **My Reservations:** View bookings
- **Invite Friends:** Add people to reservation

### 5. **🎫 QR APIs** - QR Code Generation
- **Order QR:** Generate pickup QR codes
- **Reservation QR:** Generate table check-in QR

### 6. **💰 Wallet APIs** - Digital Wallet
- **Wallet Payment:** Pay using wallet balance
- **Wallet History:** View wallet transactions

---

## 🔐 **API Security**

### **Authentication Required:**
```http
ApiKey: 30ML-LcV9mdJvKhbupWn1xOGexe4mByO
Content-Type: application/json
```

### **JWT Tokens for QR Codes:**
- **Secure:** Encrypted tokens with expiration
- **Purpose:** Prevent QR code fraud
- **Usage:** Order pickup and table check-in

---

## 📊 **Standard Response Format**

**All APIs return this structure:**
```json
{
  "status": {
    "statusCode": 200,
    "statusMessage": "Success message"
  },
  "data": {
    // Actual response data here
  }
}
```

**Status Codes:**
- ✅ `200` - Success
- ❌ `400` - Bad Request (validation error)
- 🔒 `401` - Unauthorized (missing API key)
- 🚫 `404` - Not Found
- 💥 `500` - Server Error

---

## 🎮 **Main API Endpoints**

### **Essential Endpoints:**
| Endpoint | Method | Purpose |
|----------|--------|---------|
| `/application/version` | GET | Check app version |
| `/cart/test/getCart` | POST | View cart items |
| `/orders/test/myOrders` | POST | View order history |
| `/payment/test/initiate-payment` | POST | Start payment |
| `/reservation/newReservation` | POST | Book table |
| `/qr/encodeRedeem` | POST | Generate QR code |

### **Base URL:**
```
http://localhost:8080
```

### **Health Check:**
```
GET /actuator/health
```

---

## 🔄 **Typical User Journey**

### **Customer Orders a Drink:**
1. 📱 **Browse Menu** → API gets drink list
2. 🛒 **Add to Cart** → `POST /cart/test/addOrUpdateCart`
3. 💳 **Make Payment** → `POST /payment/test/initiate-payment`
4. 📋 **Order Created** → System processes order
5. 🎫 **Get QR Code** → `POST /qr/encodeRedeem`
6. 🍺 **Pickup Drink** → Scan QR at pub

### **Customer Books Table:**
1. 🪑 **Choose Date/Time** → Check availability
2. 📝 **Create Reservation** → `POST /reservation/newReservation`
3. 👥 **Invite Friends** → `POST /reservation/newInvite`
4. 🎫 **Get QR Code** → `POST /qr/encodeReservation`
5. 🏃‍♂️ **Check In** → Scan QR at pub

---

## 🛠️ **For Developers**

### **Testing APIs:**
```bash
# Using curl
curl -X POST http://localhost:8080/cart/test/getCart \
  -H "Content-Type: application/json" \
  -H "ApiKey: 30ML-LcV9mdJvKhbupWn1xOGexe4mByO" \
  -d '{"userId":"123","pubId":"456"}'

# Using Postman
# 1. Set method to POST
# 2. Add headers: ApiKey and Content-Type
# 3. Add JSON body with required parameters
```

### **Common Request Body Patterns:**
```json
// User identification
{
  "userId": "123"
}

// Cart operations
{
  "userId": "123",
  "drinkId": "789",
  "quantity": "2",
  "pubId": "456"
}

// Payment initiation
{
  "userId": "123",
  "amount": "600",
  "currency": "INR"
}
```

---

## 🎯 **Key Benefits of This API Design**

### **For Mobile App Developers:**
- ✅ **Consistent Format:** All responses follow same structure
- ✅ **Clear Errors:** Detailed error messages for debugging
- ✅ **Secure:** API key authentication and JWT tokens
- ✅ **Well Documented:** Complete request/response examples

### **For Business:**
- ✅ **Scalable:** Can handle multiple apps (iOS, Android, Web)
- ✅ **Flexible:** Easy to add new features
- ✅ **Reliable:** Proper error handling and validation
- ✅ **Integrated:** Works with Razorpay, AWS, MySQL

---

## 📖 **Next Steps**

1. **📚 Read Full Documentation:** `House_of_30mL_API_Documentation.md`
2. **🏗️ Understand Architecture:** `Project_Structure_Guide.md`
3. **🧪 Test APIs:** Use Postman or curl commands
4. **🔧 Set Up Development:** Follow setup instructions
5. **💬 Ask Questions:** Contact development team for support

---

## 💡 **Remember**

**APIs are the bridge between your mobile app and the backend system.** They handle all the complex business logic, database operations, and integrations so your app can focus on providing a great user experience! 🌉

**Think of this Order Service as the "brain" of the House of 30mL ecosystem** - it knows how to process orders, handle payments, manage reservations, and coordinate everything needed to run a successful pub business! 🧠🍺
