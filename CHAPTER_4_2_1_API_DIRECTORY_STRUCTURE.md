# CHƯƠNG 4. THIẾT KẾ HỆ THỐNG
## 4.2 THIẾT KẾ API
### 4.2.1 Cấu Trúc Thư Mục của API

#### Tổng Quan Cấu Trúc

Hệ thống Jewelry E-commerce sử dụng kiến trúc **Node.js + Express** với mô hình **MVC** (Model-View-Controller) mở rộng. Cấu trúc thư mục API được tổ chức theo các module chức năng, thuận tiện cho việc bảo trì, mở rộng và quản lý code.

```
server/src/
├── server.js                    # File entry point, cấu hình Express & Socket.io
├── config/                      # Cấu hình ứng dụng
│   ├── connectDB.js            # Kết nối MongoDB
│   ├── sendEmail.js            # Cấu hình Nodemailer
│   ├── payos.config.js         # Cấu hình PayOS
│   ├── openai.js               # Cấu hình OpenAI API
│   └── configCloudDinary.js    # Cấu hình Cloudinary
├── routes/                      # Định nghĩa các endpoint API (22 routes)
│   ├── auth.route.js           # Authentication endpoints
│   ├── user.route.js           # User management endpoints
│   ├── product.route.js        # Product endpoints
│   ├── category.route.js       # Category endpoints
│   ├── subcategory.route.js    # Subcategory endpoints
│   ├── brand.route.js          # Brand endpoints
│   ├── review.route.js         # Product review endpoints
│   ├── cart.route.js           # Shopping cart endpoints
│   ├── order.route.js          # Order processing endpoints
│   ├── payment.route.js        # Payment endpoints
│   ├── custom.route.js         # Custom order endpoints
│   ├── coupon.route.js         # Coupon/discount endpoints
│   ├── compare.route.js        # Product comparison endpoints
│   ├── wish.route.js           # Wishlist endpoints
│   ├── conversation.route.js   # Real-time chat endpoints
│   ├── chatBox.route.js        # Chatbot endpoints
│   ├── material.route.js       # Material attributes endpoints
│   ├── gemstone.route.js       # Gemstone attributes endpoints
│   ├── item.route.js           # Item/Variant endpoints
│   ├── file.route.js           # File upload endpoints
│   ├── provinces.route.js      # Location/Provinces endpoints
│   └── dashboard.route.js      # Admin dashboard endpoints
├── controller/                  # Business logic & request handlers (25+ controllers)
│   ├── auth.controller.js      # Authentication logic
│   ├── user.controller.js      # User management logic
│   ├── product.controller.js   # Product logic
│   ├── category.controller.js  # Category management logic
│   ├── review.controller.js    # Review management logic
│   ├── cart.controller.js      # Cart operations logic
│   ├── order.controller.js     # Order processing logic
│   ├── payment.controller.js   # Payment processing logic
│   ├── custom.controller.js    # Custom order logic
│   ├── coupon.controller.js    # Coupon logic
│   ├── compare.controller.js   # Comparison logic
│   ├── wish.controller.js      # Wishlist logic
│   ├── conservation.controller.js   # Conversation logic
│   ├── chatBox.controller.js   # Chatbot logic
│   ├── material.controller.js  # Material attribute logic
│   ├── gemstone.controller.js  # Gemstone attribute logic
│   ├── item.controller.js      # Item/Variant logic
│   ├── file.controller.js      # File handling logic
│   ├── dashboard.controller.js # Dashboard stats logic
│   ├── ai.controller.js        # AI features logic
│   ├── base.controller.js      # Base controller (parent class)
│   └── [other controllers...]
├── models/                      # Mongoose schemas & models (15 models)
│   ├── user.model.js           # User schema
│   ├── product.model.js        # Product schema
│   ├── category.model.js       # Category schema
│   ├── subcategory.model.js    # Subcategory schema
│   ├── brand.model.js          # Brand schema
│   ├── review.model.js         # Review schema
│   ├── cart.model.js           # Cart schema
│   ├── order.model.js          # Order schema
│   ├── custom.model.js         # Custom order schema
│   ├── coupon.model.js         # Coupon schema
│   ├── compare.model.js        # Comparison schema
│   ├── wish.model.js           # Wishlist schema
│   ├── conversation.model.js   # Conversation schema
│   ├── material.model.js       # Material schema
│   ├── gemstone.model.js       # Gemstone schema
│   ├── session.model.js        # Session schema
│   └── ai.model.js             # AI conversation schema
├── services/                    # Business logic layer (18 services)
│   ├── auth.service.js         # Authentication services
│   ├── user.service.js         # User services
│   ├── product.service.js      # Product services
│   ├── category.service.js     # Category services
│   ├── subcategory.service.js  # Subcategory services
│   ├── brand.service.js        # Brand services
│   ├── review.service.js       # Review services
│   ├── cart.service.js         # Cart services
│   ├── order.service.js        # Order services
│   ├── payment.service.js      # Payment services
│   ├── custom.service.js       # Custom order services
│   ├── coupon.service.js       # Coupon services
│   ├── compare.service.js      # Comparison services
│   ├── wish.service.js         # Wishlist services
│   ├── conservation.service.js # Chat services
│   ├── material.service.js     # Material services
│   ├── gemstone.service.js     # Gemstone services
│   ├── token.service.js        # JWT token services
│   ├── dashboard.service.js    # Dashboard services
│   └── ai.service.js           # AI services
├── Schema/                      # Zod validation schemas (8 schemas)
│   ├── auth.schema.js          # Authentication validation
│   ├── user.schema.js          # User validation
│   ├── product.schema.js       # Product validation
│   ├── category.schema.js      # Category validation
│   ├── subcategory.schema.js   # Subcategory validation
│   ├── brand.shema.js          # Brand validation
│   ├── coupon.shema.js         # Coupon validation
│   └── commonSchema.js         # Common validations
├── auth/                        # Authentication middleware
│   ├── checkAuth.js            # JWT verification middleware
│   └── checkRole.js            # Role-based access control
├── middleware/                  # Express middleware
│   └── middleware.js           # Custom middleware (CORS, logging, etc.)
├── libs/                        # Utility functions & libraries
│   ├── rateLimit.js            # Rate limiting configuration
│   ├── toSlug.js               # URL slug conversion
│   ├── publicId.js             # Cloudinary public ID extractor
│   ├── estimateCustom.js       # Custom order estimation
│   └── calcPrice.js            # Price calculation utilities
├── AI/                          # AI features
│   └── parseIntent.js          # Intent parsing for chatbot
├── core/                        # Core utilities
│   └── error.response.js       # Error handling & response formatting
├── swagger.js                   # API documentation (Swagger/OpenAPI)
└── test.js                      # Testing file

```

#### Chi Tiết Các Thành Phần Chính

**1. Routes Layer (22 API routes)**
- Định nghĩa các endpoint REST API
- Mapping HTTP methods (GET, POST, PUT, DELETE, PATCH)
- Gọi các controller tương ứng
- Áp dụng middleware xác thực & phân quyền

**2. Controllers Layer (25+ controllers)**
- Xử lý request từ client
- Gọi các service layer để xử lý logic
- Format response để gửi về client
- Xử lý validation request

**3. Services Layer (18+ services)**
- Chứa core business logic
- Tương tác với database thông qua models
- Tính toán, xử lý dữ liệu phức tạp
- Gọi các external API (PayOS, OpenAI, Cloudinary)

**4. Models Layer (15 models)**
- Định nghĩa Mongoose schemas
- Đại diện cho MongoDB collections
- Chứa pre/post hooks
- Indexes & validation rules

**5. Config Layer**
- Cấu hình kết nối database
- Cấu hình external services (PayOS, OpenAI, Cloudinary, Nodemailer)
- Environment variables

**6. Middleware & Auth**
- JWT authentication middleware
- Role-based authorization (checkRole)
- Rate limiting
- CORS configuration

**7. Utils & Libs**
- Helper functions
- Validation utilities
- Calculation functions

#### Flow Xử Lý Request

```
Client Request
     ↓
Routes (route matching)
     ↓
Middleware (CORS, Auth, Rate limit)
     ↓
Controller (request validation, response formatting)
     ↓
Service (business logic, calculations)
     ↓
Model (database queries)
     ↓
MongoDB
     ↓
Response back to Client
```

#### Bảng Tóm Tắt Các Thành Phần

| Thành Phần | Số Lượng | Chức Năng | Vị Trí |
|------------|----------|----------|--------|
| Routes | 22 | Định nghĩa API endpoints | `routes/` |
| Controllers | 25+ | Xử lý requests | `controller/` |
| Services | 18+ | Business logic | `services/` |
| Models | 15 | Database schemas | `models/` |
| Validation Schemas | 8 | Data validation (Zod) | `Schema/` |
| Configs | 5 | External service configs | `config/` |
| Middlewares | 2 | Auth & access control | `auth/`, `middleware/` |
| Utils/Libs | 5 | Helper functions | `libs/`, `core/` |
| Socket.io Events | Multiple | Real-time communication | `server.js` |

#### Các Route Chính (22 API Routes)

1. **Authentication**: `/api/auth` - Đăng nhập, đăng ký
2. **Users**: `/api/users` - Quản lý user profile
3. **Products**: `/api/product` - Quản lý sản phẩm
4. **Categories**: `/api/category` - Quản lý danh mục
5. **SubCategories**: `/api/subcategory` - Quản lý danh mục con
6. **Brands**: `/api/brand` - Quản lý thương hiệu
7. **Reviews**: `/api/review` - Đánh giá sản phẩm
8. **Cart**: `/api/cart` - Giỏ hàng
9. **Orders**: `/api/order` - Đơn hàng
10. **Payment**: `/api/payment` - Thanh toán
11. **Custom Orders**: `/api/custom` - Đơn hàng tùy chỉnh
12. **Coupons**: `/api/coupon` - Mã giảm giá
13. **Comparison**: `/api/compare` - So sánh sản phẩm
14. **Wishlist**: `/api/wish` - Danh sách yêu thích
15. **Chat**: `/api/chat` - Real-time conversation
16. **Chatbot**: `/api/chat-bot` - Chatbot tự động
17. **Materials**: `/api/material` - Vật liệu
18. **Gemstones**: `/api/gemstone` - Đá quý
19. **Items**: `/api/items` - Biến thể sản phẩm
20. **Files**: `/api/file` - Upload file
21. **Provinces**: `/api/provinces` - Danh sách địa phương
22. **Dashboard**: `/api/dashboard` - Thống kê admin

#### Dependencies Giữa Các Layer

```
Routes
   ↓
Controllers → Services → Models → MongoDB
   ↓
Middleware (Auth, Rate limit)

External APIs:
- PayOS (Payment)
- OpenAI (AI/Chatbot)
- Cloudinary (Image storage)
- Nodemailer (Email)
- Google OAuth (Authentication)
```

#### Ghi Chú Bảo Mật & Best Practices

1. **Authentication**: Sử dụng JWT token, lưu trữ trong HTTP-only cookies
2. **Authorization**: Role-based access control (checkRole middleware)
3. **Validation**: Sử dụng Zod schemas để validate request data
4. **Rate Limiting**: Giới hạn số request để chống spam/attack
5. **CORS**: Cấu hình để chỉ cho phép origin từ client
6. **Helmet**: Security headers configuration
7. **Password**: Mã hóa bcrypt trước khi lưu
8. **Error Handling**: Centralized error response class

---

**Kết Luận**: Cấu trúc thư mục API được thiết kế theo nguyên tắc **separation of concerns**, giúp code sạch, dễ bảo trì, và dễ mở rộng. Mỗi layer có trách nhiệm rõ ràng, từ route handling đến database operations.
