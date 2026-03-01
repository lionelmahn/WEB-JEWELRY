# BIỂU ĐỒ KIẾN TRÚC HỆ THỐNG JEWELRY E-COMMERCE

## 1. KIẾN TRÚC N-LAYER TỔNG QUÁT

```
╔════════════════════════════════════════════════════════════════════╗
║                    PRESENTATION LAYER                              ║
║  ┌─────────────┬──────────────┬─────────────┬────────────────┐    ║
║  │   Admin     │   User       │   Public    │   Mobile App   │    ║
║  │ Dashboard   │  Account     │  Homepage   │   (React)      │    ║
║  └─────────────┴──────────────┴─────────────┴────────────────┘    ║
╚═══════════════════════════════════════════════════════════════════╝
            ↓↑ (HTTP/HTTPS, JSON)
╔════════════════════════════════════════════════════════════════════╗
║              API GATEWAY & ROUTING LAYER (Express.js)              ║
║  ┌──────────────────────────────────────────────────────────┐    ║
║  │  app.use() - 22 Route Middlewares                        │    ║
║  │  /api/auth, /api/product, /api/order, /api/payment...   │    ║
║  │                                                          │    ║
║  │  Middleware Stack:                                       │    ║
║  │  • CORS + Helmet (Security)                             │    ║
║  │  • Rate Limiting (authApiLimiter, globalLimiter)       │    ║
║  │  • Authentication (checkAuth middleware)                │    ║
║  │  • Error Handling                                       │    ║
║  └──────────────────────────────────────────────────────────┘    ║
╚═══════════════════════════════════════════════════════════════════╝
            ↓↑ (Method Calls, Service Pattern)
╔════════════════════════════════════════════════════════════════════╗
║         APPLICATION & BUSINESS LOGIC LAYER                        ║
║                    (Controllers & Services)                        ║
║  ┌──────────────────────────────────────────────────────────┐    ║
║  │ PRODUCT CATALOG SUBSYSTEMS                              │    ║
║  │ ┌─────────────┬──────────────┬──────────────────────┐   │    ║
║  │ │ ProductCtrl │ CategoryCtrl  │ BrandCtrl, etc.     │   │    ║
║  │ └─────────────┴──────────────┴──────────────────────┘   │    ║
║  │ SHOPPING & ORDERING SUBSYSTEMS                         │    ║
║  │ ┌─────────────┬──────────────┬──────────────────────┐   │    ║
║  │ │ CartCtrl    │ OrderCtrl     │ CustomOrderCtrl     │   │    ║
║  │ │ PaymentCtrl │                                     │   │    ║
║  │ └─────────────┴──────────────┴──────────────────────┘   │    ║
║  │ ENGAGEMENT & FEATURE SUBSYSTEMS                         │    ║
║  │ ┌─────────────┬──────────────┬──────────────────────┐   │    ║
║  │ │ ReviewCtrl  │ WishCtrl      │ CouponCtrl          │   │    ║
║  │ │ CompareCtrl │                                     │   │    ║
║  │ └─────────────┴──────────────┴──────────────────────┘   │    ║
║  │ COMMUNICATION & MANAGEMENT SUBSYSTEMS                   │    ║
║  │ ┌─────────────┬──────────────┬──────────────────────┐   │    ║
║  │ │ ChatCtrl    │ UserCtrl      │ DashboardCtrl       │   │    ║
║  │ │ AICtrl      │                                     │   │    ║
║  │ └─────────────┴──────────────┴──────────────────────┘   │    ║
║  └──────────────────────────────────────────────────────────┘    ║
╚═══════════════════════════════════════════════════════════════════╝
            ↓↑ (Query/Update, Mongoose)
╔════════════════════════════════════════════════════════════════════╗
║               DATA ACCESS LAYER (Mongoose ORM)                    ║
║  ┌──────────────────────────────────────────────────────────┐    ║
║  │ MODEL DEFINITIONS & SCHEMAS                             │    ║
║  │ ┌──────────────────────────────────────────────────┐    │    ║
║  │ │ User   Product   Order    Cart      Custom        │    │    ║
║  │ │ Review Wish      Compare  Coupon    Category      │    │    ║
║  │ │ Brand  Material  Gemstone Item      Conversation  │    │    ║
║  │ └──────────────────────────────────────────────────┘    │    ║
║  │ • Schema Validation                                     │    ║
║  │ • Pre/Post Hooks (auto timestamp, password hashing)    │    ║
║  │ • Indexes (performance optimization)                    │    ║
║  │ • Aggregation Pipelines (complex queries)              │    ║
║  └──────────────────────────────────────────────────────────┘    ║
╚═══════════════════════════════════════════════════════════════════╝
            ↓↑ (MongoDB Queries)
╔════════════════════════════════════════════════════════════════════╗
║                    DATABASE LAYER                                 ║
║              MongoDB (NoSQL Document Store)                       ║
║  ┌──────────────────────────────────────────────────────────┐    ║
║  │ Collections:                                             │    ║
║  │ • users, products, orders, carts, customs, reviews      │    ║
║  │ • wishes, compares, coupons, categories, brands         │    ║
║  │ • materials, gemstones, items, conversations            │    ║
║  └──────────────────────────────────────────────────────────┘    ║
╚═══════════════════════════════════════════════════════════════════╝

GIAO DIỆN BỔ SUNG:
┌────────────────────────────────────────────────────────────────────┐
│ • WebSocket Layer (Socket.io)                                      │
│   - Real-time chat between customers and admins                    │
│   - Events: join-room, send-message, receive-message, typing      │
│   - Order status updates (real-time notifications)                 │
│                                                                    │
│ • Authentication Layer (JWT Tokens)                                │
│   - Token generation on login                                      │
│   - Token verification on protected routes                         │
│   - Session management via cookies                                 │
│                                                                    │
│ • External Services Integration                                    │
│   - PayOS Payment Gateway (Webhook support)                        │
│   - Google OAuth (future)                                          │
│                                                                    │
│ • File Storage Layer (Multer)                                      │
│   - Product image uploads                                          │
│   - User avatar uploads                                            │
│   - Design files for custom orders                                 │
└────────────────────────────────────────────────────────────────────┘
```

---

## 2. CHI TIẾT CÁC LAYER

### **LAYER 1: PRESENTATION LAYER (Client-Side)**
```
React Application
├── Pages
│   ├── HomePage          (Product listing, search)
│   ├── ProductDetail     (Single product view)
│   ├── Cart              (Shopping cart)
│   ├── Checkout          (Order confirmation)
│   ├── OrderHistory      (User orders)
│   ├── CustomOrder       (Custom jewelry request)
│   ├── UserAccount       (Profile, settings)
│   ├── Wishlist          (Saved products)
│   ├── Compare           (Product comparison)
│   └── AdminDashboard    (Admin panel)
│
├── Components
│   ├── ProductCard
│   ├── FilterBar
│   ├── CartSummary
│   ├── PaymentForm
│   ├── ChatWindow
│   └── ReviewModal
│
└── State Management
    ├── Redux Store
    ├── API calls (axios/fetch)
    └── WebSocket client (socket.io)
```

### **LAYER 2: API GATEWAY & ROUTING LAYER**
```
Express Server (server.js)
├── Middleware Stack
│   ├── CORS ({ origin: http://localhost:5173 })
│   ├── Helmet (Security headers)
│   ├── Express JSON parser
│   ├── Cookie Parser
│   └── Rate Limiter
│
└── Route Modules (22 files)
    ├── auth.route.js           → /api/auth
    ├── product.route.js        → /api/product
    ├── cart.route.js           → /api/cart
    ├── order.route.js          → /api/order
    ├── payment.route.js        → /api/payment
    ├── custom.route.js         → /api/custom
    ├── review.route.js         → /api/review
    ├── wish.route.js           → /api/wish
    ├── compare.route.js        → /api/compare
    ├── coupon.route.js         → /api/coupon
    ├── conversation.route.js   → /api/chat
    ├── user.route.js           → /api/users
    ├── category.route.js       → /api/category
    ├── subcategory.route.js    → /api/subcategory
    ├── brand.route.js          → /api/brand
    ├── material.route.js       → /api/material
    ├── gemstone.route.js       → /api/gemstone
    ├── item.route.js           → /api/items
    ├── dashboard.route.js      → /api/dashboard
    ├── chatBox.route.js        → /api/chat-bot
    ├── file.route.js           → /api/file
    └── provinces.route.js      → /api/provinces

Middleware Chains
├── Global: globalLimiter
├── Auth endpoints: authApiLimiter
├── Protected: authUser (JWT verification)
└── Role-based: checkRole (Admin/User)
```

### **LAYER 3: APPLICATION & BUSINESS LOGIC LAYER**
```
Controllers (22 files)
├── auth.controller.js        → signup, login, logout
├── product.controller.js     → CRUD products, search, filter
├── cart.controller.js        → add/remove items, calculate total
├── order.controller.js       → create order, update status
├── payment.controller.js     → process payment, webhook
├── custom.controller.js      → create custom order, approve design
├── review.controller.js      → create/update reviews
├── wish.controller.js        → add/remove wishlist
├── compare.controller.js     → compare products
├── coupon.controller.js      → validate coupons, apply discount
├── conversation.controller.js → fetch/create conversations
├── chatBox.controller.js     → AI chatbot responses
├── user.controller.js        → manage user profile
├── category.controller.js    → CRUD categories
├── subcategory.controller.js → CRUD subcategories
├── brand.controller.js       → CRUD brands
├── material.controller.js    → CRUD materials
├── gemstone.controller.js    → CRUD gemstones
├── item.controller.js        → CRUD items
├── dashboard.controller.js   → analytics, reports
├── provinces.controller.js  → get province/district/ward
└── ai.controller.js          → AI endpoints

Services Layer (Business Logic)
├── productService.js         → Product queries, filtering
├── orderService.js           → Order processing logic
├── paymentService.js         → Payment calculation
├── customService.js          → Custom order workflow
├── cartService.js            → Cart calculations
├── couponService.js          → Discount calculations
└── ...others

Validation & Security
├── checkAuth.js              → JWT verification
├── roleChecker.js            → Role-based access control
├── validators.js             → Input validation
└── errorResponse.js          → Error handling
```

### **LAYER 4: DATA ACCESS LAYER (Mongoose ORM)**
```
Models (15 files)
├── user.model.js             → User schema (15 fields)
├── product.model.js          → Product schema (variants, images)
├── order.model.js            → Order schema (items, status, payment)
├── cart.model.js             → Cart schema (items, quantities)
├── custom.model.js           → Custom Order schema (design, status)
├── review.model.js           → Review schema (rating, comment)
├── wish.model.js             → Wishlist schema
├── compare.model.js          → Comparison schema
├── coupon.model.js           → Coupon schema (code, discount)
├── category.model.js         → Category schema
├── subcategory.model.js      → SubCategory schema
├── brand.model.js            → Brand schema
├── material.model.js         → Material schema
├── gemstone.model.js         → Gemstone schema
├── item.model.js             → Item schema
└── conversation.model.js     → Conversation schema (messages)

Schema Features
├── Validation Rules          → Required fields, unique, enum
├── Indexes                   → Performance optimization
├── Pre/Post Hooks           → Auto timestamps, password hashing
├── Relationships            → References (ObjectId), nested objects
└── Aggregation Pipelines    → Complex queries for dashboard
```

### **LAYER 5: DATABASE LAYER**
```
MongoDB Collections (15+)
├── users              (User accounts, profiles)
├── products           (Product catalog)
├── orders             (Customer orders)
├── carts              (Shopping carts)
├── customs            (Custom orders)
├── reviews            (Product reviews)
├── wishes             (Wishlists)
├── compares           (Comparisons)
├── coupons            (Discount codes)
├── categories         (Product categories)
├── subcategories      (Sub-categories)
├── brands             (Brands)
├── materials          (Materials for products)
├── gemstones          (Gemstone types)
├── items              (Product variants)
└── conversations      (Chat messages)

Database Features
├── Indexes             → Fast queries
├── TTL Indexes        → Auto-expiring data (sessions)
├── Transactions       → Multi-document ACID transactions
└── Replication Set    → High availability
```

---

## 3. PHỤ THUỘC GIỮA CÁC HỆ THỐNG CON

```
┌─────────────────────────────────────────────────────────────┐
│ TRÍCH DẪN: Authentication Subsystem                         │
│ • Khi login thành công → Cấp JWT token                     │
│ • Token được lưu trong Cookie hoặc LocalStorage            │
│ • Mỗi request chứa token trong header                      │
│ • Middleware checkAuth verify token trước khi xử lý       │
└─────────────────────────────────────────────────────────────┘

Authentication → Authorization
    ↓                ↓
    └──→ All Subsystems (require authenticated user)


Product Catalog (Category, SubCategory, Brand, Material, Gemstone, Item)
    ↓
Shopping Cart → nhập sản phẩm vào giỏ
    ↓
Order Processing → tạo đơn hàng từ giỏ
    ↓
Payment Processing → thanh toán đơn hàng
    ↓
Review & Rating → sau khi đơn giao (completed)


Parallel Path:
Custom Order → Design Approval → Order Processing → Payment → Chat support


Wishlist & Comparison
    ↓
    └──→ sử dụng Product data (không ảnh hưởng đến Order flow)


Coupon
    ↓
    └──→ applied trong Order Processing & Custom Order


Communication (Chat)
    ↓
    └──→ used for Order support & Custom Order negotiation


User Management
    ↓
    └──→ referenced by All subsystems


Dashboard & Analytics
    ↓
    └──→ aggregate data từ Order, Custom, User, Review (read-only)
```

---

## 4. QUYỀN TRUY CẬP THEO ROLE

```
PUBLIC (No Authentication)
├── GET /api/product              (Browse products)
├── GET /api/category             (View categories)
├── GET /api/brand                (View brands)
├── POST /api/auth/login          (Login)
└── POST /api/auth/signup         (Register)

AUTHENTICATED USER
├── All PUBLIC endpoints
├── POST /api/cart/add            (Add to cart)
├── GET /api/orders               (View my orders)
├── POST /api/order               (Create order)
├── POST /api/payment/create-link (Create payment link)
├── POST /api/review              (Leave review)
├── POST /api/wish/add            (Add to wishlist)
├── POST /api/compare/add         (Compare products)
├── GET /api/custom               (View custom orders)
├── POST /api/custom              (Create custom order)
├── GET /api/chat                 (Chat with shop)
└── PUT /api/users/profile        (Update profile)

ADMIN
├── All USER endpoints
├── POST /api/product             (Create product)
├── PUT /api/product/:id          (Update product)
├── DELETE /api/product/:id       (Delete product)
├── PUT /api/order/:id/status     (Update order status)
├── PUT /api/custom/:id/approve   (Approve custom design)
├── POST /api/coupon              (Create coupon)
├── GET /api/dashboard            (View analytics)
├── PATCH /api/category/:id       (Manage categories)
└── POST /api/chat/admin          (Respond to customers)
```

---

## 5. COMMUNICATION FLOW

### **Flow 1: Standard Order (User → Admin → Delivery)**
```
1. User Browse Products
   GET /api/product → ProductController → Product Model

2. User Add to Cart
   POST /api/cart/add → CartController → Cart Model

3. User Checkout
   POST /api/order → OrderController → Order Model
                  + Apply Coupon → CouponController
                  + Calculate Total → Service Layer

4. User Make Payment
   POST /api/payment/create-link → PaymentController
                                 → PayOS API → Webhook

5. Shop Process Order
   PUT /api/order/:id/status → OrderController → Order Model

6. Shop Send Notification
   Socket.io emit 'order-status-update'

7. User Review After Delivery
   POST /api/review → ReviewController → Review Model

8. Dashboard Analytics
   GET /api/dashboard → DashboardController → aggregation
```

### **Flow 2: Custom Order (User → Chat → Design Approval → Admin → Delivery)**
```
1. User Browse Custom Order Section
   GET /api/custom/form-template

2. User Create Custom Order
   POST /api/custom → CustomController → Custom Model

3. User Chat with Shop
   Socket.io: 'customer_message' → Conversation Model
           ← 'receive-message' (Admin response)

4. Admin Approve Design
   PUT /api/custom/:id/approve → CustomController
                               → Conversation (notification)

5. User Make Payment
   POST /api/payment/create-link

6. Shop Manufacture & Send
   PUT /api/order/:id/status (linked to custom order)

7. User Review
   POST /api/review
```

---

## 6. SECURITY MEASURES

```
Layer 2 (API Gateway)
├── CORS (whitelist only http://localhost:5173)
├── Helmet (security headers)
├── Rate Limiting
│   ├── authApiLimiter (15 requests per 15 min)
│   └── globalLimiter (100 requests per 15 min)
└── HTTPS (in production)

Layer 3 (Business Logic)
├── JWT Token Verification
├── Role-based Access Control
├── Input Validation
├── SQL Injection Prevention (Mongoose parameterized)
├── XSS Prevention
└── Error Message Sanitization

Layer 4 (Data Access)
├── Schema Validation
├── Indexes on sensitive fields (email, unique)
├── Password Hashing (bcrypt pre-hook)
└── Data Encryption (PII if needed)

Database
├── MongoDB Authentication
├── Network Security (IP whitelisting)
├── Backup & Recovery
└── Audit Logging
```

---

## 7. SCALABILITY CONSIDERATIONS

```
Current Architecture (Single Server):
├── Express Server (single instance)
├── MongoDB (single node)
└── Socket.io (in-memory)

Future Scaling:
├── Load Balancer (Nginx/HAProxy)
├── Multiple Express Instances
├── Database Replication Set
├── Redis for caching
├── Redis Pub/Sub for Socket.io scaling
├── CDN for static files
└── Microservices (Payment, Chat, Analytics separate)
```

---

## KẾT LUẬN

Hệ thống được thiết kế theo **Clean Architecture** với:
- **5 Layers** rõ ràng (Presentation, API Gateway, Business Logic, Data Access, Database)
- **23 Subsystems** độc lập nhưng liên kết
- **RESTful API** + **WebSocket** cho real-time features
- **Role-based Access Control** cho bảo mật
- **Service Pattern** cho logic tái sử dụng
- **Mongoose ORM** cho database abstraction

Kiến trúc này cho phép:
✓ Dễ dàng thêm feature mới (tạo subsystem mới)
✓ Dễ kiểm thử (mock dependencies)
✓ Dễ bảo trì (rõ ràng boundary)
✓ Dễ scale (horizontal scaling of API & DB)
✓ Dễ hiểu (layered structure)
