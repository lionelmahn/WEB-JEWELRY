## 4.2.1 Cấu Trúc Thư Mục của API

### Tổng Quan Kiến Trúc RESTful API

Hệ thống Jewelry sử dụng kiến trúc RESTful API với mô hình Resource-oriented, được thiết kế theo nguyên tắc CRUD (Create, Read, Update, Delete). Kiến trúc được tổ chức theo các lớp:

```
┌─────────────────────────────────────────────────────────────────┐
│                    APPLICATION LAYER                             │
│  (React Components, Pages, State Management)                    │
└─────────────────────┬───────────────────────────────────────────┘
                      │
                      │ REST API Calls (HTTP)
                      ▼
┌─────────────────────────────────────────────────────────────────┐
│                  API LAYER (Node.js/Express)                    │
│  ┌──────────────────────────────────────────────────────────┐  │
│  │  Routes Layer                                            │  │
│  │  - /api/auth              - /api/products               │  │
│  │  - /api/users             - /api/orders                 │  │
│  │  - /api/cart              - /api/custom                 │  │
│  │  - /api/reviews           - /api/coupons                │  │
│  │  - /api/wishlist          - /api/conversation           │  │
│  └──────────────────────────────────────────────────────────┘  │
│                      ▼                                           │
│  ┌──────────────────────────────────────────────────────────┐  │
│  │  Middleware Layer                                        │  │
│  │  - Authentication (checkRole, isAuthenticated)          │  │
│  │  - Validation (Schema validation middleware)            │  │
│  │  - Rate Limiting (loginLimiter, refreshLimiter)         │  │
│  │  - Error Handling (Global error handler)                │  │
│  └──────────────────────────────────────────────────────────┘  │
│                      ▼                                           │
│  ┌──────────────────────────────────────────────────────────┐  │
│  │  Controllers Layer                                       │  │
│  │  - userController        - productController           │  │
│  │  - orderController       - customController            │  │
│  │  - cartController        - reviewController            │  │
│  │  - couponController      - wishController              │  │
│  │  - conservationController                              │  │
│  └──────────────────────────────────────────────────────────┘  │
│                      ▼                                           │
│  ┌──────────────────────────────────────────────────────────┐  │
│  │  Services/Business Logic Layer                          │  │
│  │  - Authentication service                              │  │
│  │  - Order processing service                            │  │
│  │  - Payment service                                     │  │
│  │  - Custom order calculation                            │  │
│  └──────────────────────────────────────────────────────────┘  │
└─────────────────────┬───────────────────────────────────────────┘
                      │ Mongoose ORM
                      ▼
┌─────────────────────────────────────────────────────────────────┐
│                  DATA ACCESS LAYER                              │
│  ┌──────────────────────────────────────────────────────────┐  │
│  │  Models Layer (Mongoose Schemas)                        │  │
│  │  - User, Product, Order, Cart, Review                  │  │
│  │  - Custom, Coupon, Wish, Conversation                  │  │
│  │  - Category, SubCategory, Brand, Material, GemStone    │  │
│  │  - Compare                                             │  │
│  └──────────────────────────────────────────────────────────┘  │
└─────────────────────┬───────────────────────────────────────────┘
                      │
                      ▼
┌─────────────────────────────────────────────────────────────────┐
│                   DATABASE LAYER                                │
│  MongoDB Collections:                                           │
│  - users, products, orders, carts, reviews, customs           │
│  - coupons, wishes, conversations, categories, etc.           │
└─────────────────────────────────────────────────────────────────┘
```

### HTTP Methods và Quy Tắc CRUD

Hệ thống sử dụng các phương thức HTTP chuẩn:

- **GET (SELECT)**: Truy vấn dữ liệu từ Resource
- **POST (CREATE)**: Tạo mới một Resource
- **PUT (UPDATE)**: Cập nhật toàn bộ hoặc một phần Resource
- **PATCH (UPDATE)**: Cập nhật một phần của Resource
- **DELETE (DELETE)**: Xoá một Resource

### Resource Model

Các API Resource được tổ chức theo các subsystems chính:

| Resource | Collection | Chức Năng |
|----------|-----------|---------|
| /api/auth | Authentication | Đăng nhập, đăng ký, refresh token |
| /api/users | User Management | Quản lý thông tin người dùng |
| /api/products | Product Catalog | Quản lý sản phẩm có sẵn |
| /api/cart | Shopping Cart | Quản lý giỏ hàng |
| /api/orders | Order Processing | Quản lý đơn hàng |
| /api/custom | Custom Order | Quản lý đơn hàng tùy chỉnh |
| /api/reviews | Review & Rating | Đánh giá sản phẩm |
| /api/wishlist | Wishlist | Danh sách yêu thích |
| /api/coupons | Coupon Management | Mã giảm giá |
| /api/conversation | Real-time Communication | Chat với admin |

### Cấu Trúc Thư Mục Server

```
server/src/
├── routes/                    # Route handlers
│   ├── auth.route.js         # Authentication routes
│   ├── user.route.js         # User management routes
│   ├── product.route.js      # Product routes
│   ├── cart.route.js         # Cart routes
│   ├── order.route.js        # Order routes
│   ├── custom.route.js       # Custom order routes
│   ├── review.route.js       # Review routes
│   ├── wish.route.js         # Wishlist routes
│   ├── coupon.route.js       # Coupon routes
│   ├── conversation.route.js # Conversation routes
│   └── payment.route.js      # Payment routes
│
├── controller/               # Business logic controllers
│   ├── user.controller.js    # User CRUD operations
│   ├── product.controller.js # Product CRUD operations
│   ├── order.controller.js   # Order processing
│   ├── custom.controller.js  # Custom order handling
│   ├── cart.controller.js    # Cart operations
│   ├── review.controller.js  # Review operations
│   ├── wish.controller.js    # Wishlist operations
│   ├── coupon.controller.js  # Coupon handling
│   └── conservation.controller.js # Conversation handling
│
├── models/                   # Mongoose schemas
│   ├── user.model.js
│   ├── product.model.js
│   ├── order.model.js
│   ├── custom.model.js
│   ├── cart.model.js
│   ├── review.model.js
│   ├── wish.model.js
│   ├── coupon.model.js
│   ├── conversation.model.js
│   ├── category.model.js
│   ├── subcategory.model.js
│   ├── brand.model.js
│   ├── material.model.js
│   ├── gemstone.model.js
│   └── compare.model.js
│
├── Schema/                   # Validation schemas
│   ├── auth.schema.js       # Auth validation
│   ├── user.schema.js       # User validation
│   ├── product.schema.js    # Product validation
│   ├── coupon.schema.js     # Coupon validation
│   └── commonSchema.js      # Common validation
│
├── middleware/              # Express middleware
│   ├── middleware.js        # Custom validation middleware
│   └── errorHandler.js      # Error handling middleware
│
├── auth/                    # Authentication utilities
│   ├── checkRole.js         # Role-based access control
│   ├── jwt.js               # JWT token handling
│   └── passport.js          # Passport configuration
│
├── libs/                    # Utility libraries
│   ├── rateLimit.js         # Rate limiting
│   ├── nodemailer.js        # Email service
│   ├── payos.js             # Payment gateway
│   └── socket.io.js         # WebSocket configuration
│
├── config/                  # Configuration files
│   ├── database.js          # MongoDB connection
│   ├── env.js               # Environment variables
│   └── constants.js         # Application constants
│
├── utils/                   # Utility functions
│   ├── response.js          # Standard response format
│   ├── validation.js        # Validation helpers
│   └── helpers.js           # Common helpers
│
└── server.js               # Main server entry point
```

### Request-Response Flow

```
Client Request (HTTP)
        ↓
Routes Layer (Express Router)
        ↓
Middleware Chain
   - Authentication
   - Authorization (Role Check)
   - Validation
   - Rate Limiting
        ↓
Controller
   - Extract request data
   - Call service/business logic
        ↓
Models/Database
   - Query/Update MongoDB
        ↓
Response to Client (JSON)
```

### Komunikasi với External Services

- **Payment**: PayOS Gateway (qua payment.route.js)
- **Email**: Nodemailer (xác thực, thông báo)
- **Authentication**: Google OAuth (sign-in-google)
- **Real-time Chat**: Socket.io (WebSocket)

### Security Measures

- JWT authentication tokens
- Role-based access control (RBAC)
- Rate limiting (loginLimiter, refreshLimiter)
- Input validation (Zod schemas)
- Password hashing (bcrypt)
- CORS configuration
- SQL injection prevention (Mongoose parameterized queries)
