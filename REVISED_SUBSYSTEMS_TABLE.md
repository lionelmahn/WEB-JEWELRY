# BẢNG MÔ TẢ CHI TIẾT CÁC HỆ THỐNG CON - JEWELRY E-COMMERCE SYSTEM

## PHÂN TÍCH CẤU TRÚC LAYER HỆ THỐNG

### Kiến Trúc N-Layer Thực Tế Của Hệ Thống

Dựa trên phân tích codebase, hệ thống được chia thành **5 Layer chính**:

```
┌──────────────────────────────────────────────────────────────────┐
│                    PRESENTATION LAYER                             │
│                      (React.js Client)                            │
│   Admin Dashboard │ User Account │ Public Homepage │ Mobile App   │
└────────────────────────┬─────────────────────────────────────────┘
                         │ (HTTP/HTTPS)
                         ↓
┌──────────────────────────────────────────────────────────────────┐
│              API GATEWAY & ROUTING LAYER                          │
│  (Express.js Routes, Authentication Middleware, Rate Limiting)    │
│  • auth.route.js, product.route.js, order.route.js, etc.         │
└────────────────────────┬─────────────────────────────────────────┘
                         │ (Request/Response)
                         ↓
┌──────────────────────────────────────────────────────────────────┐
│         APPLICATION & BUSINESS LOGIC LAYER                        │
│  (Controllers, Services, Business Rules, Validation)              │
│  • Controllers: ProductController, OrderController, etc.          │
│  • Services: productService, orderService, etc.                   │
│  • Validation: checkAuth middleware, role-based authorization     │
└────────────────────────┬─────────────────────────────────────────┘
                         │ (Method Calls)
                         ↓
┌──────────────────────────────────────────────────────────────────┐
│              DATA ACCESS LAYER (Models & ORM)                     │
│  (Mongoose Schemas, Database Queries, Aggregations)              │
│  • Models: User, Product, Order, Cart, Custom, etc. (15 models)  │
│  • Database Transactions & Validations                            │
└────────────────────────┬─────────────────────────────────────────┘
                         │ (Queries)
                         ↓
┌──────────────────────────────────────────────────────────────────┐
│                     DATABASE LAYER                                │
│                   MongoDB (Document Store)                        │
│           Collections: users, products, orders, etc.              │
└──────────────────────────────────────────────────────────────────┘

Giao diện bổ sung:
├─ WebSocket Layer (Socket.io) - Real-time Chat & Notifications
├─ Authentication Layer (JWT Tokens, Cookie-based Sessions)
├─ External Services (PayOS Payment Gateway)
└─ File Storage Layer (Multer - Disk Upload)
```

---

## BẢNG MÔ TẢ CHI TIẾT 16 HỆ THỐNG CON

### **LAYER 1: APPLICATION & API GATEWAY LAYER**

| STT | Tên Hệ Thống Con | Layer | Mô Tả Chức Năng | Routes Chính | Controllers | Models |
|-----|------------------|-------|-----------------|--------------|-------------|--------|
| 1 | Authentication Subsystem | Application/Gateway | Xác thực người dùng (login/signup), JWT token management, phiên làm việc | `/api/auth` | authController | User |
| 2 | Authorization & Middleware Subsystem | Application/Gateway | Kiểm soát quyền truy cập (role-based), rate limiting, CORS | checkAuth, globalLimiter | middleware | User (role field) |
| 3 | API Gateway & Routing | Application/Gateway | Định tuyến request, middleware chaining, error handling, CORS | 22 routes (auth, product, order, etc.) | app.use() | N/A |

---

### **LAYER 2: BUSINESS LOGIC LAYER - PRODUCT CATALOG SUBSYSTEMS**

| STT | Tên Hệ Thống Con | Layer | Mô Tả Chức Năng | Routes Chính | Controllers | Models |
|-----|------------------|-------|-----------------|--------------|-------------|--------|
| 4 | Product Management Subsystem | Business Logic | Quản lý sản phẩm có sẵn: CRUD, search, filter, promotion | `/api/product` | ProductController | Product, variant fields |
| 5 | Category Management Subsystem | Business Logic | Quản lý danh mục sản phẩm | `/api/category` | CategoryController | Category |
| 6 | Subcategory Management Subsystem | Business Logic | Quản lý danh mục con (nested categories) | `/api/subcategory` | SubcategoryController | SubCategory |
| 7 | Brand Management Subsystem | Business Logic | Quản lý thương hiệu sản phẩm | `/api/brand` | BrandController | Brand |
| 8 | Material & Gemstone Subsystem | Business Logic | Quản lý vật liệu và đá quý (Chi tiết sản phẩm) | `/api/material`, `/api/gemstone` | MaterialController, GemstoneController | Material, Gemstone |
| 9 | Item Management Subsystem | Business Logic | Quản lý các mục/biến thể sản phẩm chi tiết | `/api/items` | ItemController | Item |

---

### **LAYER 2: BUSINESS LOGIC LAYER - SHOPPING & ORDERING SUBSYSTEMS**

| STT | Tên Hệ Thống Con | Layer | Mô Tả Chức Năng | Routes Chính | Controllers | Models |
|-----|------------------|-------|-----------------|--------------|-------------|--------|
| 10 | Shopping Cart Subsystem | Business Logic | Quản lý giỏ hàng: add, remove, update quantity, calculate total | `/api/cart` | CartController | Cart, CartItem |
| 11 | Order Processing Subsystem | Business Logic | Xử lý đặt hàng, quản lý trạng thái đơn hàng (pending, shipped, delivered) | `/api/order` | OrderController | Order |
| 12 | Custom Order Subsystem | Business Logic | Quản lý đơn hàng tùy chỉnh: thiết kế riêng, phê duyệt, sản xuất | `/api/custom` | CustomController | Custom |
| 13 | Payment Processing Subsystem | Business Logic | Xử lý thanh toán: tích hợp PayOS, webhook, xác minh giao dịch | `/api/payment` | PaymentController | Order (payment fields) |

---

### **LAYER 2: BUSINESS LOGIC LAYER - FEATURE & ENGAGEMENT SUBSYSTEMS**

| STT | Tên Hệ Thống Con | Layer | Mô Tả Chức Năng | Routes Chính | Controllers | Models |
|-----|------------------|-------|-----------------|--------------|-------------|--------|
| 14 | Review & Rating Subsystem | Business Logic | Quản lý đánh giá sản phẩm, bình luận, tính điểm trung bình | `/api/review` | ReviewController | Review |
| 15 | Wishlist & Comparison Subsystem | Business Logic | Danh sách yêu thích, so sánh sản phẩm | `/api/wish`, `/api/compare` | WishController, CompareController | Wish, Compare |
| 16 | Coupon & Promotion Subsystem | Business Logic | Quản lý mã giảm giá, khuyến mãi, tính toán giảm giá | `/api/coupon` | CouponController | Coupon |

---

### **LAYER 2: BUSINESS LOGIC LAYER - COMMUNICATION & MANAGEMENT SUBSYSTEMS**

| STT | Tên Hệ Thống Con | Layer | Mô Tả Chức Năng | Routes Chính | Controllers | Models |
|-----|------------------|-------|-----------------|--------------|-------------|--------|
| 17 | Real-time Communication Subsystem | Business Logic + WebSocket | Chat real-time giữa khách hàng và shop, thông báo | `/api/chat`, Socket.io events | ConversationController, ChatBoxController | Conversation, Message |
| 18 | User Management Subsystem | Business Logic | Quản lý hồ sơ người dùng, thông tin cá nhân, xóa tài khoản | `/api/users` | UserController | User |
| 19 | Dashboard & Analytics Subsystem | Business Logic | Thống kê cho admin: bán hàng, đơn hàng, người dùng | `/api/dashboard` | DashboardController | Order, User, Custom (aggregation) |

---

### **LAYER 3: DATA ACCESS LAYER**

| STT | Tên Hệ Thống Con | Layer | Mô Tả Chức Năng | Routes Chính | Controllers | Models |
|-----|------------------|-------|-----------------|--------------|-------------|--------|
| 20 | Database Management Subsystem | Data Access | Truy xuất dữ liệu từ MongoDB: queries, transactions, indexes, validation | N/A | Mongoose ORM | 15 models (User, Product, Order, Cart, etc.) |

---

### **LAYER 4: UTILITIES & EXTERNAL SERVICES LAYER**

| STT | Tên Hệ Thống Con | Layer | Mô Tả Chức Năng | Routes Chính | Controllers | Models |
|-----|------------------|-------|-----------------|--------------|-------------|--------|
| 21 | File Storage & Upload Subsystem | Utilities | Quản lý upload hình ảnh sản phẩm, avatar người dùng | `/api/file` | (Multer middleware) | N/A |
| 22 | Location & Address Subsystem | Utilities | Quản lý tỉnh/thành phố, quận/huyện, phường/xã | `/api/provinces` | ProvincesController | N/A (Static data) |
| 23 | AI Chatbot Subsystem | Utilities | Hỗ trợ khách hàng thông qua AI chatbot | `/api/chat-bot` | AIController | N/A (API integration) |

---

## PHÂN TÍCH PHỤ THUỘC & TƯƠNG TÁC GIỮA CÁC SUBSYSTEM

### **Hierarchy của Layer:**

```
Presentation Layer (React Client)
         ↓
    HTTP Requests
         ↓
API Gateway & Routing Layer
    (Express + Middleware)
         ↓
Application & Business Logic Layer
    (22 Controllers + Services)
         ↓
Data Access Layer
    (Mongoose Models)
         ↓
MongoDB Database
```

### **Phụ Thuộc Chức Năng Giữa Các Subsystem:**

```
1. Authentication → Gating (bảo vệ)
   ↓
2. Authorization → Access Control

3. Product Catalog (Category, SubCategory, Brand, Material, Gemstone, Item)
   ↓
4. Shopping Cart → sử dụng Product data
   ↓
5. Order Processing → sử dụng Cart + Product + Coupon
   ↓
6. Payment Processing → Complete Order

7. Custom Order → Parallel với Order, sử dụng Design Approval

8. Review & Rating → phải có Order completed
   ↓
9. Wishlist & Comparison → sử dụng Product data

10. Communication (Chat) → cho cả Order lẫn Custom Order support
    ↓
11. User Management → hỗ trợ tất cả subsystem

12. Dashboard → Aggregate data từ Order, Custom, User, Review
```

---

## BẢNG TÓMS TẮT SUBSYSTEM VỚI DEPENDENCIES

| # | Subsystem | Chức Năng | Phụ Thuộc Vào | Dữ Liệu Chính | HTTP Method |
|---|-----------|----------|---------------|--------------|------------|
| 1 | Authentication | Đăng nhập/Đăng ký | - | User | POST |
| 2 | Authorization | Kiểm soát quyền | Authentication | User.role | Middleware |
| 3 | Product Management | CRUD sản phẩm | Authorization | Product | GET/POST/PUT/DELETE |
| 4 | Category | CRUD danh mục | Authorization | Category | GET/POST/PUT/DELETE |
| 5 | Subcategory | CRUD danh mục con | Category | SubCategory | GET/POST/PUT/DELETE |
| 6 | Brand | CRUD thương hiệu | Authorization | Brand | GET/POST/PUT/DELETE |
| 7 | Material | CRUD vật liệu | Authorization | Material | GET/POST/PUT/DELETE |
| 8 | Gemstone | CRUD đá quý | Authorization | Gemstone | GET/POST/PUT/DELETE |
| 9 | Item | CRUD mục chi tiết | Authorization | Item | GET/POST/PUT/DELETE |
| 10 | Shopping Cart | Quản lý giỏ | Authentication, Product | Cart, CartItem | POST/DELETE/GET |
| 11 | Order Processing | Đặt hàng | Cart, User, Coupon | Order | POST/GET/PUT |
| 12 | Custom Order | Đơn tùy chỉnh | Authentication, User | Custom | POST/GET/PUT |
| 13 | Payment Processing | Thanh toán | Order/Custom, PayOS | Transaction | POST, Webhook |
| 14 | Review & Rating | Đánh giá | Order (completed) | Review | POST/GET/PUT |
| 15 | Wishlist | Danh sách yêu thích | Product, User | Wish | POST/DELETE/GET |
| 16 | Comparison | So sánh sản phẩm | Product | Compare | POST/DELETE/GET |
| 17 | Coupon | Mã giảm giá | Authorization (Admin) | Coupon | GET/POST/PUT |
| 18 | Communication | Chat real-time | Authentication, Order/Custom | Conversation, Message | WebSocket + REST |
| 19 | User Management | Quản lý hồ sơ | Authentication | User | GET/PUT/DELETE |
| 20 | Dashboard | Thống kê | Authorization (Admin) | Order, Custom, User, Review | GET |
| 21 | File Upload | Tải hình ảnh | Authentication | File Path | POST |
| 22 | Location | Địa chỉ | - | Province, District, Ward | GET |
| 23 | AI Chatbot | Hỗ trợ AI | Authentication | External API | POST |

---

## PHÂN TÍCH LAYER CHÍNH XÁC

### **1. PRESENTATION LAYER (React.js Client)**
- **Nhiệm vụ:** Hiển thị giao diện, thu thập dữ liệu từ người dùng
- **Thành phần:** Pages, Components, State Management (Redux/Context)
- **Giao tiếp:** HTTP requests qua API Gateway

### **2. API GATEWAY & ROUTING LAYER (Express.js)**
- **Nhiệm vụ:** Tiếp nhận request, định tuyến, apply middleware, CORS
- **Thành phần:** 22 route files (auth, product, order, etc.)
- **Middleware chính:** 
  - Authentication: `checkAuth.js` (JWT verification)
  - Rate Limiting: `rateLimit.js`
  - CORS: cross-origin requests
  - Error Handling

### **3. APPLICATION & BUSINESS LOGIC LAYER (Controllers + Services)**
- **Nhiệm vụ:** Xử lý logic kinh doanh, validation, orchestration
- **Thành phần:** 
  - 22 Controllers (ProductController, OrderController, etc.)
  - Service layer (productService, orderService, etc.)
  - Validation logic
  - Authorization checks (Role-based: Admin, User)
- **Nhiệm vụ chính:**
  - Validate input
  - Apply business rules
  - Orchestrate cross-subsystem operations
  - Handle exceptions

### **4. DATA ACCESS LAYER (Mongoose ORM)**
- **Nhiệm vụ:** Định nghĩa schema, query, aggregation, transaction
- **Thành phần:** 15 Mongoose Models
  - User, Product, Order, Cart, Custom, Review, Wish, Compare
  - Category, SubCategory, Brand, Material, Gemstone, Item, Coupon, Conversation
- **Tính năng:**
  - Schema validation (Mongoose validators)
  - Indexes (tối ưu query)
  - Pre/Post hooks (auto timestamp, password hashing)
  - Aggregation pipelines (complex queries)

### **5. DATABASE LAYER**
- **Nhiệm vụ:** Lưu trữ dữ liệu
- **Công nghệ:** MongoDB (NoSQL Document Store)
- **Đặc điểm:** Flexible schema, horizontal scaling

---

## TỔNG HỢP: 23 HỆ THỐNG CON

**Application Layer (3):**
- Authentication, Authorization, API Gateway & Routing

**Business Logic Layer (17):**
- Product Management, Category, Subcategory, Brand
- Material & Gemstone, Item
- Shopping Cart, Order Processing, Custom Order, Payment
- Review & Rating, Wishlist & Comparison, Coupon
- Communication, User Management, Dashboard & Analytics

**Data Access Layer (1):**
- Database Management Subsystem

**Utilities & External Services (2):**
- File Upload, Location, AI Chatbot

---

## KẾT LUẬN

Hệ thống Jewelry E-commerce được thiết kế theo **N-Layer Architecture** với 5 layer chính:
1. **Presentation Layer** - React.js Client
2. **API Gateway & Routing** - Express.js Routes & Middleware
3. **Business Logic** - Controllers + Services (17 subsystems)
4. **Data Access** - Mongoose ORM
5. **Database** - MongoDB

Kiến trúc này đảm bảo:
✓ Tách biệt rõ ràng giữa các layer (Separation of Concerns)
✓ Dễ bảo trì, mở rộng, kiểm thử
✓ Reusable components & services
✓ Xử lý lỗi tập trung
✓ Bảo mật dữ liệu & authorization rõ ràng
✓ Real-time communication qua WebSocket
✓ Tích hợp thanh toán bên thứ 3 (PayOS)
