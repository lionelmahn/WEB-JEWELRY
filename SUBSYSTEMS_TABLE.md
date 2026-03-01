# BẢNG MÔ TẢ CÁC HỆ THỐNG CON - JEWELRY E-COMMERCE SYSTEM

## Kiến Trúc N-Layer

```
┌─────────────────────────────────────────────────────────────────┐
│                     PRESENTATION LAYER                           │
│  ┌──────────────┬────────────────┬──────────────┐                │
│  │  Admin Views │   User Views   │  Public Views│                │
│  │  (Dashboard) │  (Account Page)│  (Homepage)  │                │
│  └──────────────┴────────────────┴──────────────┘                │
└─────────────────────────────────────────────────────────────────┘
            ↑                                            ↑
┌─────────────────────────────────────────────────────────────────┐
│                   APPLICATION LAYER                              │
│  ┌──────────────┬────────────────┬──────────────┐                │
│  │Authentication│ Authorization  │ Validation   │                │
│  │  Subsystem   │  Subsystem     │  Subsystem   │                │
│  └──────────────┴────────────────┴──────────────┘                │
└─────────────────────────────────────────────────────────────────┘
            ↓
┌─────────────────────────────────────────────────────────────────┐
│                  BUSINESS LOGIC LAYER                            │
│  ┌──────────────┬────────────────┬──────────────┐                │
│  │  Product     │   Order        │  Custom      │                │
│  │  Management  │   Processing   │  Order       │                │
│  │  Subsystem   │   Subsystem    │  Subsystem   │                │
│  └──────────────┴────────────────┴──────────────┘                │
│          ↓              ↓              ↓                          │
│  ┌──────────────┬────────────────┬──────────────┐                │
│  │  Payment     │   Cart         │  Catalog     │                │
│  │  Processing  │   Management   │  Management  │                │
│  │  Subsystem   │   Subsystem    │  Subsystem   │                │
│  └──────────────┴────────────────┴──────────────┘                │
│          ↓              ↓              ↓                          │
│  ┌──────────────┬────────────────┬──────────────┐                │
│  │  Wishlist &  │   Review &     │  Communication
│  │  Comparison  │   Rating       │  Subsystem   │                │
│  │  Subsystem   │   Subsystem    │  (WebSocket) │                │
│  └──────────────┴────────────────┴──────────────┘                │
│          ↓              ↓              ↓                          │
│  ┌────────────────────────────────────────────┐                 │
│  │    Coupon & User Management Subsystem      │                 │
│  └────────────────────────────────────────────┘                 │
└─────────────────────────────────────────────────────────────────┘
            ↓
┌─────────────────────────────────────────────────────────────────┐
│                   DATA ACCESS LAYER                              │
│  ┌────────────────────────────────────────────┐                 │
│  │   Database Management Subsystem             │                 │
│  │   (MongoDB + Mongoose ORM)                  │                 │
│  └────────────────────────────────────────────┘                 │
└─────────────────────────────────────────────────────────────────┘
```

---

## BẢNG MÔ TẢ CHI TIẾT CÁC HỆ THỐNG CON

### **PRESENTATION LAYER**

| STT | Tên Hệ Thống Con | Layer | Mô Tả Chức Năng | Components Chính |
|-----|------------------|-------|-----------------|-------------------|
| 1 | Admin Dashboard Views | Presentation | Hiển thị thống kê, quản lý hàng hóa, đơn hàng, người dùng | DashboardPage, OrderManagement, ProductManagement, UserManagement |
| 2 | User Account Views | Presentation | Hiển thị thông tin tài khoản, lịch sử mua hàng, danh sách yêu thích | AccountPage, OrderHistory, WishlistPage, CustomOrderHistory |
| 3 | Public Views | Presentation | Giao diện công khai - trang chủ, danh sách sản phẩm, chi tiết sản phẩm | HomePage, ProductCatalog, ProductDetail, CartPage |

---

### **APPLICATION LAYER**

| STT | Tên Hệ Thống Con | Layer | Mô Tả Chức Năng | Components Chính |
|-----|------------------|-------|-----------------|-------------------|
| 4 | Authentication Subsystem | Application | Xác thực người dùng, quản lý session, JWT token | AuthController, LoginRouter, RegisterRouter, Middleware: VerifyToken |
| 5 | Authorization Subsystem | Application | Phân quyền, kiểm soát truy cập dựa trên vai trò (User, Admin, Seller) | RoleMiddleware, PermissionChecker, Policies: AdminPolicy, SellerPolicy |
| 6 | Validation Subsystem | Application | Validate dữ liệu form, business rules, input validation | ValidationMiddleware, RequestValidators: RegisterRequest, ProductRequest, OrderRequest |

---

### **BUSINESS LOGIC LAYER**

| STT | Tên Hệ Thống Con | Layer | Mô Tả Chức Năng | Components Chính |
|-----|------------------|-------|-----------------|-------------------|
| 7 | Product Management Subsystem | Business Logic | Quản lý sản phẩm có sẵn, giá cả, hình ảnh, kho hàng | ProductController, ProductModel, ProductService, BrandController, CategoryController |
| 8 | Catalog Management Subsystem | Business Logic | Quản lý phân loại sản phẩm: danh mục, thương hiệu, vật liệu, đá quý, subcategory | CategoryController, BrandController, MaterialController, GemstoneController, SubcategoryController |
| 9 | Cart Management Subsystem | Business Logic | Quản lý giỏ hàng, thêm/xóa sản phẩm, tính tổng giá | CartController, CartModel, CartService, CartItemModel |
| 10 | Custom Order Subsystem | Business Logic | Quản lý đơn hàng tùy chỉnh, yêu cầu khách hàng, bản thiết kế | CustomController, CustomModel, CustomService, DesignApprovalService |
| 11 | Order Processing Subsystem | Business Logic | Xử lý quy trình đặt hàng, quản lý trạng thái đơn hàng, tracking | OrderController, OrderModel, OrderService, OrderStatusManager, OrderTrackingService |
| 12 | Payment Processing Subsystem | Business Logic | Xử lý thanh toán, tích hợp PayOS, xác minh giao dịch, hoàn tiền | PaymentController, PayOSService, TransactionModel, PaymentStatusManager |
| 13 | Wishlist & Comparison Subsystem | Business Logic | Quản lý danh sách yêu thích, so sánh sản phẩm | WishController, WishModel, CompareController, CompareModel |
| 14 | Review & Rating Subsystem | Business Logic | Quản lý đánh giá, bình luận sản phẩm, tính điểm trung bình | ReviewController, ReviewModel, ReviewService, RatingCalculator |
| 15 | Communication Subsystem | Business Logic | Quản lý chat real-time, trao đổi thông tin giữa khách và shop | ConversationController, ConversationModel, MessageModel, SocketIO Handler |
| 16 | Coupon & Promotion Subsystem | Business Logic | Quản lý mã giảm giá, khuyến mãi, tính toán giảm giá | CouponController, CouponModel, PromotionService, DiscountCalculator |
| 17 | User Management Subsystem | Business Logic | Quản lý thông tin người dùng, hồ sơ, quyền truy cập | UserController, UserModel, UserService, ProfileService |

---

### **DATA ACCESS LAYER**

| STT | Tên Hệ Thống Con | Layer | Mô Tả Chức Năng | Components Chính |
|-----|------------------|-------|-----------------|-------------------|
| 18 | Database Management Subsystem | Data Access | Truy xuất, lưu trữ, cập nhật dữ liệu từ MongoDB | Mongoose ORM, Database Models (User, Product, Order, Cart, etc.), Database Connection, MongoDB Indexes, Aggregation Pipelines |

---

## PHÂN TÍCH PHỤ THUỘC GIỮA CÁC SUBSYSTEM

### **Phụ Thuộc Trong Application Layer:**
```
Authentication Subsystem
    ↓
Authorization Subsystem ← Validation Subsystem
```

### **Phụ Thuộc Trong Business Logic Layer:**

```
Product Management ←─┐
      ↓              │
Catalog Management   │
      ↓              │
  ┌─────────────────┘
  │
Cart Management ──→ Order Processing ──→ Payment Processing
  │                       ↓                      ↓
  │              Wishlist & Comparison    Transaction Logging
  │                       ↓
  │              Review & Rating
  ↓
Custom Order ──→ DesignApproval ──→ Order Processing
      ↓
Communication
      ↓
   [All subsystems can use this for notifications]
      ↓
Coupon & Promotion ──→ [Used in Order & Custom Order]
      ↓
User Management ──→ [Referenced by all subsystems]
```

---

## INTERFACE CONTRACTS

### **REST API Interfaces**
```
Authentication:
  POST   /api/auth/register
  POST   /api/auth/login
  POST   /api/auth/logout
  GET    /api/auth/profile

Product:
  GET    /api/products
  GET    /api/products/:id
  POST   /api/products (Admin)
  PUT    /api/products/:id (Admin)

Order:
  POST   /api/orders
  GET    /api/orders/:id
  PUT    /api/orders/:id/status (Admin)
  GET    /api/orders (User)

Payment:
  POST   /api/payment/create-link
  POST   /api/payment/webhook (PayOS)

Cart:
  POST   /api/cart/add
  DELETE /api/cart/remove/:id
  GET    /api/cart

Custom Order:
  POST   /api/custom-order
  PUT    /api/custom-order/:id/approve
  GET    /api/custom-order/:id

Conversation:
  GET    /api/conversations
  POST   /api/conversations
  GET    /api/conversations/:id/messages
```

### **WebSocket Events**
```
Socket.IO Events:
  on  'join-room'           → Join conversation
  emit 'new-message'         → Send message
  on  'receive-message'      → Receive message
  emit 'typing'              → User is typing
  on  'order-status-update'  → Order status changed
```

### **Database Interfaces**
```
MongoDB Collections:
  users
  products
  orders
  carts
  customs
  reviews
  conversations
  messages
  coupons
  wishes
  compares
  brands
  categories
  gemstones
  materials
  subcategories
```

---

## BẢNG TÓMS TẮT CHỨC NĂNG THEO SUBSYSTEM

| Subsystem | Chức Năng Chính | Yêu Cầu Phụ Thuộc | Dữ Liệu Chính |
|-----------|-----------------|-------------------|---------------|
| Authentication | Đăng nhập, đăng ký, JWT | Validation | User |
| Authorization | Kiểm soát quyền | Authentication | User.role |
| Product Mgmt | CRUD sản phẩm | Authorization | Product, Brand, Category |
| Cart | Quản lý giỏ | Product, User | Cart, CartItem |
| Order | Xử lý đơn hàng | Cart, Coupon, User | Order |
| Payment | Thanh toán | Order | Transaction |
| Custom Order | Đơn tùy chỉnh | User, Design Approval | Custom |
| Review | Đánh giá | Order, Product | Review |
| Wishlist | Danh sách yêu thích | Product, User | Wish |
| Communication | Chat real-time | User, Order/Custom | Conversation, Message |
| Coupon | Mã giảm giá | Admin Mgmt | Coupon |
| User Mgmt | Quản lý người dùng | Authentication | User |

---

## KẾT LUẬN

Hệ thống Jewelry E-commerce được chia thành **18 subsystem** phân tán trên **4 layer**:
- **3 subsystem** trong Application Layer (Authentication, Authorization, Validation)
- **12 subsystem** trong Business Logic Layer (quản lý sản phẩm, đơn hàng, thanh toán, v.v.)
- **3 subsystem** trong Presentation Layer (Admin, User, Public Views)
- **1 subsystem** trong Data Access Layer (Database Management)

Kiến trúc này đảm bảo:
✓ Tách biệt rõ ràng giữa các lớp
✓ Dễ bảo trì và mở rộng
✓ Tái sử dụng code hiệu quả
✓ Xử lý lỗi tập trung
✓ Bảo mật dữ liệu tốt
