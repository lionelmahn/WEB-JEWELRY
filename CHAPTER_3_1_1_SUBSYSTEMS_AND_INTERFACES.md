## CHƯƠNG 3. THIẾT KẾ (USE-CASE DESIGN)

### 3.1 Xác định các thành phần thiết kế (Identify design elements)

#### 3.1.1 Xác định các hệ thống con và giao diện (Identify subsystems and interfaces)

---

### I. TỔNG QUAN KIẾN TRÚC HỆ THỐNG

Hệ thống Jewelry E-Commerce được thiết kế theo mô hình **N-Layer Architecture** với 5 tầng chính:

```
┌────────────────────────────────────────────────────────────────────┐
│                    PRESENTATION LAYER                              │
│               (React.js Client - Frontend)                         │
│   Admin Dashboard │ User Account │ Public Homepage │ Mobile        │
└─────────────────────────┬──────────────────────────────────────────┘
                          │ (HTTP/HTTPS, JSON)
                          ↓
┌────────────────────────────────────────────────────────────────────┐
│              API GATEWAY & ROUTING LAYER                           │
│                  (Express.js Route)                                │
│  22 Routes: /api/auth, /api/product, /api/order, /api/payment...  │
│  Middleware: CORS, Rate Limiting, Authentication, Error Handler   │
└─────────────────────────┬──────────────────────────────────────────┘
                          │ (Request/Response)
                          ↓
┌────────────────────────────────────────────────────────────────────┐
│         APPLICATION & BUSINESS LOGIC LAYER                         │
│               (Controllers, Services, Logic)                       │
│  22 Controllers: ProductCtrl, OrderCtrl, CustomCtrl, PaymentCtrl..│
│  Services: productService, orderService, paymentService, etc.     │
└─────────────────────────┬──────────────────────────────────────────┘
                          │ (Method Calls, Service Pattern)
                          ↓
┌────────────────────────────────────────────────────────────────────┐
│               DATA ACCESS LAYER (Mongoose ORM)                     │
│                  (Models & Database Schemas)                       │
│  15 Models: User, Product, Order, Cart, Custom, Review, etc.      │
│  Validation, Indexing, Pre/Post Hooks, Aggregation               │
└─────────────────────────┬──────────────────────────────────────────┘
                          │ (MongoDB Queries)
                          ↓
┌────────────────────────────────────────────────────────────────────┐
│                    DATABASE LAYER                                  │
│              (MongoDB NoSQL Document Store)                        │
│       Collections: users, products, orders, carts, customs...     │
└────────────────────────────────────────────────────────────────────┘
```

**Giao diện bổ sung:**
- **WebSocket Layer (Socket.io)**: Real-time chat, notifications
- **Authentication Layer (JWT Tokens)**: Session management
- **External Services**: PayOS Payment Gateway
- **File Storage (Multer)**: Image uploads

---

### II. BẢNG MÔ TẢ CHI TIẾT CÁC HỆ THỐNG CON

| STT | Tên Hệ Thống Con | Layer | Mô Tả Chức Năng | Components Chính |
|-----|------------------|-------|-----------------|------------------|
| **1** | **Authentication Subsystem** | Application | Xác thực người dùng, quản lý session, JWT token | LoginController, authController, checkAuth middleware, User model |
| **2** | **Authorization Subsystem** | Application | Phân quyền, kiểm soát truy cập (role-based access) | checkRole middleware, authorize middleware, Policies |
| **3** | **API Gateway & Routing** | Application | Định tuyến request, middleware chaining, error handling | 22 Route modules, app.use(), middleware stack |
| **4** | **Product Management Subsystem** | Business Logic | Quản lý sản phẩm có sẵn: CRUD, search, filter | ProductController, productService, Product model |
| **5** | **Category Management Subsystem** | Business Logic | Quản lý danh mục sản phẩm | CategoryController, Category model |
| **6** | **Subcategory Management Subsystem** | Business Logic | Quản lý danh mục con (phân cấp) | SubcategoryController, SubCategory model |
| **7** | **Brand Management Subsystem** | Business Logic | Quản lý thương hiệu sản phẩm | BrandController, Brand model |
| **8** | **Material & Gemstone Subsystem** | Business Logic | Quản lý vật liệu và đá quý chi tiết | MaterialController, GemstoneController, Material model, Gemstone model |
| **9** | **Shopping Cart Subsystem** | Business Logic | Quản lý giỏ hàng: add, remove, update quantity | CartController, cartService, Cart model, CartItem |
| **10** | **Order Processing Subsystem** | Business Logic | Xử lý đặt hàng, quản lý trạng thái (pending→delivered) | OrderController, orderService, Order model |
| **11** | **Custom Order Subsystem** | Business Logic | Quản lý đơn hàng tùy chỉnh: thiết kế riêng, phê duyệt | CustomController, customService, Custom model |
| **12** | **Payment Processing Subsystem** | Business Logic | Xử lý thanh toán, tích hợp PayOS, webhook validation | PaymentController, paymentService, Order (payment fields) |
| **13** | **Review & Rating Subsystem** | Business Logic | Quản lý đánh giá sản phẩm, bình luận, rating | ReviewController, Review model |
| **14** | **Wishlist & Comparison Subsystem** | Business Logic | Danh sách yêu thích, so sánh sản phẩm | WishController, CompareController, Wish model, Compare model |
| **15** | **Coupon & Promotion Subsystem** | Business Logic | Quản lý mã giảm giá, tính toán discount | CouponController, Coupon model |
| **16** | **Real-time Communication Subsystem** | Business Logic | Chat real-time, notifications | ConversationController, Socket.io handlers, Conversation model |
| **17** | **User Management Subsystem** | Business Logic | Quản lý tài khoản người dùng, profile | UserController, userService, User model |
| **18** | **Dashboard & Analytics Subsystem** | Business Logic | Thống kê, báo cáo cho admin | DashboardController, aggregation queries |
| **19** | **Database Management Subsystem** | Data Access | Truy xuất và lưu trữ dữ liệu | Mongoose ORM, 15 Models, Schemas, Indexes |
| **20** | **File Management Subsystem** | Data Access | Xử lý upload hình ảnh, file | Multer middleware, FileController, disk storage |

---

### III. CHI TIẾT GIAO DIỆN (INTERFACES) CỦA MỖI HỆ THỐNG CON

#### **3.1 Authentication Subsystem**

**Giao diện cung cấp (Provided Interfaces):**
```
POST   /api/sign-up              Input: {email, password, name} → Output: {token, user}
POST   /api/sign-in              Input: {email, password} → Output: {token, user}
POST   /api/sign-in-google        Input: {googleToken} → Output: {token, user}
POST   /api/refresh               Input: {refreshToken} → Output: {newToken}
POST   /logout                    Input: {token} → Output: {success}
```

**Giao diện yêu cầu (Required Interfaces):**
- User Model (read/write)
- Token generation service
- Password hashing service (bcrypt)

---

#### **3.2 Product Management Subsystem**

**Giao diện cung cấp:**
```
GET    /api/product              → [Product] (danh sách)
GET    /api/product/:id          → Product (chi tiết)
GET    /api/product/search       → [Product] (tìm kiếm)
POST   /api/product              → Product (tạo mới - admin)
PUT    /api/product/:id          → Product (cập nhật - admin)
DELETE /api/product/:id          → {success} (xóa - admin)
POST   /api/product/upload       → {imageUrl} (upload ảnh)
```

**Giao diện yêu cầu:**
- Category, Brand, Material, Gemstone models
- File storage service (Multer)
- Image processing service

---

#### **3.3 Shopping Cart Subsystem**

**Giao diện cung cấp:**
```
GET    /api/cart                 → Cart {items: [], total, count}
POST   /api/cart                 → Cart (add to cart)
PUT    /api/cart/:itemId         → Cart (update quantity)
DELETE /api/cart/:itemId         → Cart (remove item)
POST   /api/cart/checkout        → {orderId, total} (prepare checkout)
```

**Giao diện yêu cầu:**
- Product model (read)
- Order model (create)
- User session/auth

---

#### **3.4 Order Processing Subsystem**

**Giao diện cung cấp:**
```
POST   /api/order                → Order {id, status: 'pending'}
GET    /api/order/:id            → Order (chi tiết đơn)
GET    /api/order/user/:userId   → [Order] (đơn của user)
PUT    /api/order/:id            → Order (cập nhật trạng thái)
GET    /api/order/status/:status → [Order] (lọc theo trạng thái)
```

**Giao diện yêu cầu:**
- Cart model (read/delete)
- Product model (update inventory)
- Payment model
- Notification service

---

#### **3.5 Custom Order Subsystem**

**Giao diện cung cấp:**
```
POST   /api/custom               → Custom {id, status: 'pending'}
POST   /api/custom/calculate     → {estimatedPrice, timeline}
GET    /api/custom/user          → [Custom] (đơn của user)
GET    /api/custom/:id           → Custom (chi tiết)
PUT    /api/custom/:id           → Custom (update status - admin)
POST   /api/custom/preview/:id   → {previewImage, details}
```

**Giao diện yêu cầu:**
- Material, Gemstone models (read)
- User model (read)
- Image generation service
- Payment model

---

#### **3.6 Payment Processing Subsystem**

**Giao diện cung cấp:**
```
POST   /api/payment              → {paymentUrl, transactionId}
POST   /api/payment/webhook      → {status} (PayOS callback)
GET    /api/payment/:orderId     → Payment {amount, status, method}
POST   /api/payment/verify       → {verified: boolean}
```

**Giao diện yêu cầu:**
- Order model (read/update)
- PayOS Gateway API
- User model (read)
- Email service (confirmation)

---

#### **3.7 Wishlist & Comparison Subsystem**

**Giao diện cung cấp:**
```
POST   /api/wish                 → Wish {userId, productId}
GET    /api/wish/user            → [Wish] (danh sách yêu thích)
DELETE /api/wish/:id             → {success}
POST   /api/compare              → Compare {products: []}
GET    /api/compare/:id          → Compare (chi tiết so sánh)
```

**Giao diện yêu cầu:**
- Product model (read)
- User authentication

---

#### **3.8 Real-time Communication Subsystem**

**Giao diện cung cấp (WebSocket Events):**
```
Socket Events:
  - join-room(userId, roomId)                    → connected
  - send-message(message, senderId, receiverId) → message-received
  - typing(userId)                               → user-typing
  - order-update(orderId, status)                → status-changed
```

**Giao diện yêu cấu:**
- Conversation model (read/write)
- User model (read)
- Order model (read)
- Socket.io server instance

---

### IV. SƠ ĐỒ PHỤ THUỘC GIỮA CÁC HỆ THỐNG CON

```
Authentication ──┐
                 ├──> API Gateway ────> Authorization
Validation ──────┘                           ↓
                                    Product Management ──┐
                                            ↓            │
                                    Category, Brand,     │
                                    Material, Gemstone   │
                                            ↓            │
                                    ┌───────────────────┘
                                    ↓
                            Shopping Cart
                                    ↓
                            Order Processing ──┐
                                    ↓           │
                            Payment Processing │
                                    ↓           │
                            Notification ──────┤
                                    ↑           │
                            Real-time Chat     │
                                    ↑           │
                            Custom Order ──────┘
                                    ↓
                            Review & Rating
                                    ↓
                            User Management
```

---

### V. BẢNG TÓMO TẮT PHỤ THUỘC (DEPENDENCY MATRIX)

| Subsystem | Phụ thuộc vào | Được phụ thuộc bởi | Dữ liệu chính |
|-----------|---------------|--------------------|------------------|
| Authentication | - | Authorization, API Gateway | User, Token |
| Authorization | Authentication | API Gateway, All Subsystems | User (role field) |
| Product Catalog | Category, Brand, Material, Gemstone | Cart, Order, Review, Wishlist | Product, attributes |
| Shopping Cart | Product, Authentication | Order Processing | Cart, CartItem |
| Order Processing | Cart, Product, Payment, User | Notification, Real-time Chat | Order, OrderItem |
| Custom Order | Material, Gemstone, User | Payment, Notification | Custom, CustomItem |
| Payment | Order, User | Order Processing | Payment Transaction |
| Review | Product, Order, User | Product (rating calculation) | Review |
| Wishlist | Product, User | - | Wish, Compare |
| Real-time Chat | User, Order, Conversation | Order Processing | Conversation, Message |
| User Management | Authentication | Order, Cart, Wish, Review | User Profile |
| Dashboard | Order, Product, User, Payment | - | Analytics Data |
| File Management | - | Product, Custom Order, User | File paths, URLs |

---

### VI. KẾT LUẬN

Hệ thống Jewelry E-Commerce gồm **20 hệ thống con** được tổ chức theo **5 lớp kiến trúc** (Presentation, Application, Business Logic, Data Access, Database). 

**Ưu điểm:**
- Tách biệt rõ ràng các concerns (SoC - Separation of Concerns)
- Dễ bảo trì và mở rộng
- Hỗ trợ real-time communication (WebSocket)
- Kiến trúc microservices-ready

**Thách thức:**
- Phụ thuộc lẫn nhau phức tạp
- Cần quản lý state giữa các subsystem
- Scaling database có thể phức tạp với MongoDB

---
