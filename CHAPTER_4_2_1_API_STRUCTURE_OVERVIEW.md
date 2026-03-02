# 4.2.1 CẤU TRÚC THƯ MỤC CỦA API

## 1. Sơ đồ Kiến Trúc RESTful API

### 1.1 Biểu đồ Tổng Quan

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                              JEWELRY SYSTEM                                  │
└─────────────────────────────────────────────────────────────────────────────┘

┌──────────────────────┐         ┌──────────────────────────┐
│   Application        │         │   Application Interface  │
│   Data Model         │────────▶│        (API)             │
│                      │         │                          │
│  - User              │         │ /api/auth               │
│  - Product           │         │ /api/user               │
│  - Order             │         │ /api/product            │
│  - Cart              │         │ /api/cart               │
│  - Custom           │         │ /api/order              │
│  - Review           │         │ /api/custom             │
│  - Wishlist         │         │ /api/review             │
│  - Coupon           │         │ /api/wish               │
│  - Conversation     │         │ /api/coupon             │
│  - Category/Brand   │         │ /api/conversation       │
│                      │         │ /api/dashboard          │
│                      │         │                          │
│                      │◀────────┤ to_resource             │
│                      │         │ from_resource           │
└──────────────────────┘         └──────────────────────────┘
           │                               │
           │                    HTTP Methods (REST)
           │                     - GET (SELECT)
           │                     - POST (CREATE)
           │                     - PUT (UPDATE)
           │                     - DELETE (DELETE)
           │                     - PATCH (PARTIAL UPDATE)
           │                               │
           ▼                               ▼
┌──────────────────────────────────────────────────────────┐
│                  HTTP Protocol Layer                      │
│                 (Application/JSON)                        │
└──────────────────────────────────────────────────────────┘
           │
           │  Request/Response over HTTPS
           │
           ▼
┌──────────────────────────────────────────────────────────┐
│                    Client Applications                    │
│                                                           │
│  - React Web App       - REST Library (Axios/Fetch)     │
│  - Mobile App          - HTTP Client Library             │
│  - Admin Dashboard     - WebSocket (Socket.io)           │
└──────────────────────────────────────────────────────────┘
```

### 1.2 REST Hoạt Động Chủ Yếu

REST hoạt động chủ yếu dựa vào giao thức HTTP. Các hoạt động cơ bản nên trên sẽ sử dụng những phương thức HTTP riêng:

- **GET (SELECT)**: Trả về một Resource hoặc một danh sách Resource.
  - Ví dụ: `GET /api/product` (lấy danh sách sản phẩm)
  - Ví dụ: `GET /api/product/:id` (lấy chi tiết sản phẩm)

- **POST (CREATE)**: Tạo mới một Resource.
  - Ví dụ: `POST /api/product` (tạo sản phẩm mới)
  - Ví dụ: `POST /api/order` (tạo đơn hàng)

- **PUT (UPDATE)**: Cập nhật thông tin cho Resource.
  - Ví dụ: `PUT /api/product/:id` (cập nhật sản phẩm)
  - Ví dụ: `PUT /api/user/:id` (cập nhật tài khoản người dùng)

- **DELETE (DELETE)**: Xoá một Resource.
  - Ví dụ: `DELETE /api/product/:id` (xoá sản phẩm)
  - Ví dụ: `DELETE /api/cart/:id` (xoá item khỏi giỏ hàng)

- **PATCH (PARTIAL UPDATE)**: Cập nhật một phần thông tin Resource.
  - Ví dụ: `PATCH /api/order/:id/status` (cập nhật trạng thái đơn hàng)

Những phương thức hay hoạt động này thường được gọi là CRUD tương ứng với Create, Read, Update, Delete – Tạo, Đọc, Sửa, Xóa.

## 2. Cấu Trúc Thư Mục Server-Side

### 2.1 Cây Thư Mục Đầy Đủ

```
server/
├── src/
│   ├── server.js                    # Entry point chính
│   │
│   ├── config/                      # Cấu hình ứng dụng
│   │   ├── database.config.js      # MongoDB configuration
│   │   ├── jwt.config.js           # JWT authentication config
│   │   ├── payos.config.js         # PayOS payment gateway
│   │   ├── multer.config.js        # File upload config
│   │   └── email.config.js         # Email service config
│   │
│   ├── middleware/                  # Middleware xử lý request
│   │   ├── auth.middleware.js      # Verify JWT token
│   │   ├── errorHandler.js         # Global error handling
│   │   ├── cors.js                 # CORS configuration
│   │   ├── rateLimiter.js          # Rate limiting
│   │   └── validation.js           # Request validation
│   │
│   ├── routes/                      # API Routes (22 routes)
│   │   ├── auth.route.js           # Authentication endpoints
│   │   ├── user.route.js           # User management endpoints
│   │   ├── product.route.js        # Product management endpoints
│   │   ├── cart.route.js           # Shopping cart endpoints
│   │   ├── order.route.js          # Order processing endpoints
│   │   ├── custom.route.js         # Custom order endpoints
│   │   ├── review.route.js         # Review & rating endpoints
│   │   ├── wish.route.js           # Wishlist endpoints
│   │   ├── coupon.route.js         # Coupon management endpoints
│   │   ├── conversation.route.js   # Real-time chat endpoints
│   │   ├── payment.route.js        # Payment processing endpoints
│   │   ├── category.route.js       # Category endpoints
│   │   ├── brand.route.js          # Brand endpoints
│   │   ├── material.route.js       # Material endpoints
│   │   ├── gemstone.route.js       # Gemstone endpoints
│   │   ├── subcategory.route.js    # Subcategory endpoints
│   │   ├── coupon.route.js         # Coupon endpoints
│   │   ├── compare.route.js        # Product comparison endpoints
│   │   ├── dashboard.route.js      # Admin dashboard endpoints
│   │   ├── file.route.js           # File upload endpoints
│   │   └── index.js                # Route aggregation
│   │
│   ├── controller/                  # Controllers (25+ files)
│   │   ├── auth.controller.js      # Auth logic (login, register, verify)
│   │   ├── user.controller.js      # User logic (profile, update info)
│   │   ├── product.controller.js   # Product logic (list, detail, search)
│   │   ├── cart.controller.js      # Cart logic (add, remove, update)
│   │   ├── order.controller.js     # Order logic (create, update, list)
│   │   ├── custom.controller.js    # Custom order logic
│   │   ├── review.controller.js    # Review logic (create, read, update)
│   │   ├── wish.controller.js      # Wishlist logic
│   │   ├── coupon.controller.js    # Coupon logic (validate, apply)
│   │   ├── conversation.controller.js  # Chat logic
│   │   ├── payment.controller.js   # Payment logic (process, webhook)
│   │   ├── category.controller.js  # Category management
│   │   ├── brand.controller.js     # Brand management
│   │   ├── material.controller.js  # Material management
│   │   ├── gemstone.controller.js  # Gemstone management
│   │   ├── subcategory.controller.js # Subcategory management
│   │   ├── compare.controller.js   # Product comparison
│   │   ├── dashboard.controller.js # Admin dashboard statistics
│   │   └── file.controller.js      # File upload handling
│   │
│   ├── models/                      # Mongoose Models (15 schemas)
│   │   ├── user.model.js           # User schema
│   │   ├── product.model.js        # Product schema
│   │   ├── order.model.js          # Order schema
│   │   ├── cart.model.js           # Cart schema
│   │   ├── custom.model.js         # Custom order schema
│   │   ├── review.model.js         # Review schema
│   │   ├── wish.model.js           # Wishlist schema
│   │   ├── coupon.model.js         # Coupon schema
│   │   ├── conversation.model.js   # Conversation schema
│   │   ├── category.model.js       # Category schema
│   │   ├── brand.model.js          # Brand schema
│   │   ├── material.model.js       # Material schema
│   │   ├── gemstone.model.js       # Gemstone schema
│   │   ├── subcategory.model.js    # Subcategory schema
│   │   └── compare.model.js        # Compare schema
│   │
│   ├── services/                    # Business Logic Services
│   │   ├── auth.service.js         # Auth business logic
│   │   ├── user.service.js         # User business logic
│   │   ├── product.service.js      # Product business logic
│   │   ├── order.service.js        # Order business logic
│   │   ├── payment.service.js      # Payment business logic
│   │   ├── email.service.js        # Email sending service
│   │   ├── sms.service.js          # SMS sending service
│   │   └── storage.service.js      # Cloud storage service
│   │
│   ├── schemas/                     # Zod Validation Schemas
│   │   ├── auth.schema.js          # Auth validation
│   │   ├── user.schema.js          # User validation
│   │   ├── product.schema.js       # Product validation
│   │   ├── order.schema.js         # Order validation
│   │   └── ...
│   │
│   ├── auth/                        # Authentication Logic
│   │   ├── jwt.auth.js             # JWT token generation/verification
│   │   ├── oauth.auth.js           # OAuth (Google) authentication
│   │   └── password.js             # Password hashing (bcrypt)
│   │
│   ├── libs/                        # External Library Wrappers
│   │   ├── payos.lib.js            # PayOS SDK wrapper
│   │   ├── nodemailer.lib.js       # Email library
│   │   ├── twilio.lib.js           # SMS library (Twilio)
│   │   ├── cloudinary.lib.js       # Image upload library
│   │   └── socket.lib.js           # Socket.io setup
│   │
│   ├── utils/                       # Utility Functions
│   │   ├── logger.js               # Logging utility
│   │   ├── errorHandler.js         # Error handling utilities
│   │   ├── validators.js           # Custom validators
│   │   ├── formatters.js           # Response formatting
│   │   ├── constants.js            # Constants & enums
│   │   ├── helpers.js              # Helper functions
│   │   └── dateUtils.js            # Date/time utilities
│   │
│   ├── sockets/                     # WebSocket (Socket.io)
│   │   ├── chat.socket.js          # Real-time chat events
│   │   └── notification.socket.js  # Real-time notifications
│   │
│   └── index.js                     # Bootstrap application

├── .env.example                     # Environment variables template
├── package.json                     # Dependencies
└── .gitignore                       # Git ignore rules
```

### 2.2 Luồng Xử Lý Request

```
CLIENT REQUEST
    │
    ▼
HTTP Request to /api/endpoint
    │
    ▼
┌─────────────────────────────┐
│   Express Middleware Stack  │
│   - CORS Handler            │
│   - Body Parser             │
│   - Auth Middleware         │
│   - Validation Middleware   │
└─────────────────────────────┘
    │
    ▼
┌─────────────────────────────┐
│   Route Handler             │
│   (routes/*.route.js)       │
│   - Kiểm tra method         │
│   - Kiểm tra tham số        │
└─────────────────────────────┘
    │
    ▼
┌─────────────────────────────┐
│   Controller                │
│   (controller/*.js)         │
│   - Xử lý logic             │
│   - Validate dữ liệu        │
│   - Gọi Service/Model       │
└─────────────────────────────┘
    │
    ▼
┌─────────────────────────────┐
│   Service Layer             │
│   (services/*.js)           │
│   - Business logic          │
│   - Data transformation     │
└─────────────────────────────┘
    │
    ▼
┌─────────────────────────────┐
│   Database Layer            │
│   (models/*.js)             │
│   - Mongoose models         │
│   - MongoDB operations      │
│   - Data persistence        │
└─────────────────────────────┘
    │
    ▼
MONGODB DATABASE
    │
    ▼ (Response)
Service → Controller → Route → Middleware → Response
    │
    ▼
JSON RESPONSE to CLIENT
```

## 3. API Response Format (Chuẩn)

### 3.1 Success Response Format

```json
{
  "success": true,
  "data": {
    // Entity data
  },
  "message": "Operation successful",
  "statusCode": 200
}
```

Hoặc cho danh sách:

```json
{
  "success": true,
  "data": [
    // Array of entities
  ],
  "pagination": {
    "total": 100,
    "page": 1,
    "limit": 10,
    "totalPages": 10
  },
  "message": "Fetched successfully",
  "statusCode": 200
}
```

### 3.2 Error Response Format

```json
{
  "success": false,
  "error": {
    "code": "INVALID_INPUT",
    "message": "Validation failed",
    "details": [
      {
        "field": "email",
        "message": "Invalid email format"
      }
    ]
  },
  "statusCode": 400
}
```

## 4. HTTP Status Codes

| Code | Meaning | Mô Tả |
|------|---------|--------|
| 200 | OK | Request thành công |
| 201 | Created | Resource được tạo thành công |
| 204 | No Content | Request thành công nhưng không có dữ liệu trả về |
| 400 | Bad Request | Dữ liệu request không hợp lệ |
| 401 | Unauthorized | Chưa xác thực hoặc token không hợp lệ |
| 403 | Forbidden | Không có quyền truy cập |
| 404 | Not Found | Resource không tìm thấy |
| 409 | Conflict | Dữ liệu xung đột (trùng lặp) |
| 422 | Unprocessable Entity | Dữ liệu không hợp lệ |
| 429 | Too Many Requests | Quá nhiều request (Rate Limit) |
| 500 | Internal Server Error | Lỗi máy chủ |
| 503 | Service Unavailable | Service không khả dụng |

## 5. Authentication & Security

### 5.1 JWT Token Flow

```
1. Client gửi username/password → POST /api/auth/login
   │
   ▼
2. Server xác thực → generate JWT token
   │
   ▼
3. Response: { accessToken, refreshToken, user }
   │
   ▼
4. Client lưu token → localStorage/sessionStorage
   │
   ▼
5. Các request sau gửi kèm: Authorization: Bearer <token>
   │
   ▼
6. Server verify token tại Auth Middleware
   │
   ▼
7. Nếu valid → tiếp tục xử lý, nếu không → 401 Unauthorized
```

### 5.2 API Authentication Requirements

- **Public endpoints**: `/api/auth/login`, `/api/auth/register`, `/api/product` (GET only)
- **Protected endpoints**: Yêu cầu JWT token trong header `Authorization: Bearer <token>`
- **Admin endpoints**: Yêu cầu role = 'admin'
- **User endpoints**: Yêu cầu token + role = 'customer' hoặc 'admin'

## 6. Tóm Tắt

Hệ thống API của Jewelry Store được xây dựng theo:
- **Architecture**: RESTful API với Node.js + Express
- **Database**: MongoDB + Mongoose
- **Routes**: 22 API route files
- **Controllers**: 20+ controller files
- **Models**: 15 Mongoose schemas
- **Services**: Business logic layer
- **Security**: JWT authentication, Role-based access control
- **Real-time**: Socket.io for instant notifications & chat
