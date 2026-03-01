# Hệ Thống Con (Subsystems) và Giao Diện (Interfaces) - Báo Cáo Chi Tiết

## 1. Tổng Quan Kiến Trúc Hệ Thống

Hệ thống Jewelry (Trang sức) được xây dựng với mô hình **N-Layer Architecture** gồm các tầng:
- **Presentation Layer (Frontend)**: React.js
- **API Gateway & Route Layer**: Express.js Route
- **Business Logic Layer**: Controller & Service
- **Data Access Layer**: MongoDB Models
- **Real-time Communication**: Socket.io

---

## 2. Xác Định Các Hệ Thống Con (Subsystems)

### 2.1 Authentication & Authorization Subsystem
**Mục đích**: Quản lý xác thực người dùng và phân quyền truy cập

**Thành phần chính:**
- Route: `/api/auth`
- Controller: `user.controller.js`
- Schema: `auth.schema.js`
- Middleware: `checkAuth.js`, `checkRole.js`
- Model: `user.model.js`

**Endpoints:**
```
POST   /api/sign-up                    - Đăng ký tài khoản
POST   /api/sign-in                    - Đăng nhập
POST   /api/sign-in-google             - Đăng nhập Google
POST   /api/refresh                    - Làm mới token
POST   /logout                         - Đăng xuất
```

**Giao diện chính:**
- Input: Email, Password, Google OAuth
- Output: JWT Token, User Info, Refresh Token

---

### 2.2 Product Management Subsystem
**Mục đích**: Quản lý sản phẩm trang sức (có sẵn)

**Thành phần chính:**
- Route: `/api/product`
- Controller: `product.controller.js`
- Schema: `product.schema.js`
- Model: `product.model.js`

**Endpoints:**
```
GET    /api/product                    - Lấy tất cả sản phẩm
GET    /api/product/:id                - Lấy chi tiết sản phẩm
GET    /api/product/edit/:id           - Lấy sản phẩm để chỉnh sửa
GET    /api/product/date-time          - Lấy sản phẩm theo thời gian
POST   /api/product                    - Tạo sản phẩm (admin)
POST   /api/product/upload             - Upload ảnh sản phẩm (admin)
POST   /api/product/file-excel         - Upload file Excel sản phẩm (admin)
POST   /api/product/file-excel-preview - Xem trước file Excel
PUT    /api/product/:id                - Cập nhật sản phẩm (admin)
DELETE /api/product/:id                - Xóa sản phẩm (admin)
DELETE /api/product/:id/image          - Xóa ảnh sản phẩm
```

**Chức năng chính:**
- CRUD sản phẩm
- Upload ảnh từ file
- Import từ Excel
- Quản lý inventory

---

### 2.3 Custom Order Subsystem
**Mục đích**: Quản lý đơn hàng tùy chỉnh (khách hàng thiết kế riêng)

**Thành phần chính:**
- Route: `/api/custom`
- Controller: `custom.controller.js`
- Model: `custom.model.js`

**Endpoints:**
```
GET    /api/custom                     - Lấy tất cả đơn tùy chỉnh (admin)
GET    /api/custom/user                - Lấy đơn tùy chỉnh của user
POST   /api/custom                     - Tạo đơn tùy chỉnh
POST   /api/custom/calculate           - Tính toán giá tùy chỉnh
POST   /api/custom/preview/:id         - Xem trước thiết kế
PUT    /api/custom/update/:id          - Cập nhật đơn tùy chỉnh
PUT    /api/custom/:id                 - Cập nhật trạng thái (admin)
```

**Giao diện:**
- Input: Design details, Materials, Gemstones, Size
- Output: Price, Preview image, Order confirmation

---

### 2.4 Shopping Cart Subsystem
**Mục đích**: Quản lý giỏ hàng người dùng

**Thành phần chính:**
- Route: `/api/cart`
- Controller: `cart.controller.js`
- Model: `cart.model.js`

**Endpoints:**
```
GET    /api/cart                       - Lấy giỏ hàng
POST   /api/cart                       - Tạo/Thêm sản phẩm vào giỏ
PATCH  /api/cart/:sku                  - Cập nhật số lượng
DELETE /api/cart/:sku                  - Xóa sản phẩm khỏi giỏ
DELETE /api/cart/clear/:sku            - Xóa toàn bộ giỏ
```

**Giao diện:**
- Input: Product ID, SKU, Quantity
- Output: Cart items, Total price, Item count

---

### 2.5 Order Processing Subsystem
**Mục đích**: Quản lý quy trình đặt hàng và xử lý đơn hàng

**Thành phần chính:**
- Route: `/api/order`
- Controller: `order.controller.js`
- Model: `order.model.js`

**Endpoints:**
```
GET    /api/order/list-order            - Lấy đơn hàng của user
GET    /api/order                       - Lấy tất cả đơn (admin)
GET    /api/order/:id                   - Chi tiết đơn hàng
POST   /api/order                       - Tạo đơn hàng
POST   /api/order/preview               - Xem trước đơn hàng
POST   /api/order/use-coupon            - Áp dụng coupon
PUT    /api/order/:id/status            - Cập nhật trạng thái (admin)
PUT    /api/order/:id/payment-status    - Cập nhật trạng thái thanh toán
DELETE /api/order/:id                   - Hủy đơn hàng
```

**Trạng thái đơn hàng:**
- Pending (Chờ xử lý)
- Confirmed (Đã xác nhận)
- Shipped (Đã giao)
- Delivered (Đã nhận)
- Cancelled (Đã hủy)

---

### 2.6 Payment Subsystem
**Mục đích**: Xử lý thanh toán cho cả đơn hàng thường và tùy chỉnh

**Thành phần chính:**
- Route: `/api/payment`
- Controller: `payment.controller.js`
- Model: (Tích hợp với Order & Custom Model)

**Endpoints:**
```
POST   /api/payment                    - Tạo link thanh toán (đơn thường)
POST   /api/payment/custom             - Tạo link thanh toán (đơn tùy chỉnh)
POST   /api/payment/success            - Callback thanh toán thành công
POST   /api/payment/success/custom     - Callback thanh toán tùy chỉnh
```

**Cổng thanh toán:**
- PayOS hoặc gateway tương tự
- Hỗ trợ QR code, chuyển khoản

---

### 2.7 Product Attributes Subsystem
**Mục đích**: Quản lý các thuộc tính sản phẩm

**Thành phần chính:**
| Subsystem | Route | Controller | Model |
|-----------|-------|------------|-------|
| Category | `/api/category` | `category.controller.js` | `category.model.js` |
| SubCategory | `/api/subcategory` | `subcategory.controller.js` | `subcategory.model.js` |
| Brand | `/api/brand` | `brand.controller.js` | `brand.model.js` |
| Material | `/api/material` | `material.controller.js` | `material.model.js` |
| Gemstone | `/api/gemstone` | `gemstone.controller.js` | `gemstone.model.js` |

**Giao diện chung:**
```
GET    /api/{resource}                 - Lấy danh sách
GET    /api/{resource}/:id             - Chi tiết
POST   /api/{resource}                 - Tạo mới (admin)
PUT    /api/{resource}/:id             - Cập nhật (admin)
DELETE /api/{resource}/:id             - Xóa (admin)
```

---

### 2.8 Comparison Subsystem
**Mục đích**: So sánh các sản phẩm

**Thành phần chính:**
- Route: `/api/compare`
- Controller: `compare.controller.js`
- Model: `compare.model.js`

**Endpoints:**
```
GET    /api/compare                    - Lấy danh sách so sánh
POST   /api/compare                    - Thêm sản phẩm vào so sánh
DELETE /api/compare/:id                - Xóa khỏi so sánh
```

---

### 2.9 Wishlist Subsystem
**Mục đích**: Quản lý danh sách yêu thích

**Thành phần chính:**
- Route: `/api/wish`
- Controller: `wish.controller.js`
- Model: `wish.model.js`

**Endpoints:**
```
GET    /api/wish                       - Lấy danh sách yêu thích
POST   /api/wish                       - Thêm vào yêu thích
DELETE /api/wish/:id                   - Xóa khỏi yêu thích
```

---

### 2.10 Review & Rating Subsystem
**Mục đích**: Quản lý đánh giá và nhận xét sản phẩm

**Thành phần chính:**
- Route: `/api/review`
- Controller: `review.controller.js`
- Model: `review.model.js`

**Endpoints:**
```
GET    /api/review                     - Lấy tất cả đánh giá
POST   /api/review                     - Tạo đánh giá
PUT    /api/review/:id                 - Cập nhật đánh giá
DELETE /api/review/:id                 - Xóa đánh giá
```

---

### 2.11 Communication Subsystem (Real-time Chat)
**Mục đích**: Trò chuyện giữa khách hàng và admin

**Thành phần chính:**
- Route: `/api/chat` (HTTP)
- Controller: `conservation.controller.js`
- Model: `conversation.model.js`
- WebSocket: Socket.io

**Endpoints (HTTP):**
```
GET    /api/chat                       - Lấy tất cả message
GET    /api/chat/me                    - Message của user hiện tại
GET    /api/chat/hasunread             - Kiểm tra có message chưa đọc
GET    /api/chat/markread              - Đánh dấu đã đọc
```

**WebSocket Events:**
```
Emitted by Client:
- customer_open_chat(userId)           - Khách hàng mở chat
- customer_message({ userId, roomId, message })
- join_admin()
- admin_message({ roomId, message })
- admin_join_room(roomId)

Emitted by Server:
- message({ roomId, from, message })
- admin_message_notify({ roomId, type })
- customers_list(list)
```

---

### 2.12 User Management Subsystem
**Mục đích**: Quản lý thông tin người dùng

**Thành phần chính:**
- Route: `/api/users`
- Controller: `user.controller.js`
- Model: `user.model.js`

**Endpoints:**
```
GET    /api/users/profile              - Lấy thông tin cá nhân
PUT    /api/users/profile              - Cập nhật thông tin
GET    /api/users                      - Lấy danh sách user (admin)
GET    /api/users/:id                  - Chi tiết user
```

---

### 2.13 Dashboard & Analytics Subsystem
**Mục đích**: Cung cấp thống kê và phân tích cho admin

**Thành phần chính:**
- Route: `/api/dashboard`
- Controller: `dashboard.controller.js`
- Models: Tích hợp từ Order, Product, User

**Endpoints:**
```
GET    /api/dashboard                  - Lấy tất cả metrics (admin)
```

**Metrics:**
- Tổng doanh thu
- Số đơn hàng
- Số khách hàng
- Top sản phẩm
- Trend bán hàng

---

### 2.14 Coupon Management Subsystem
**Mục đích**: Quản lý mã giảm giá và khuyến mãi

**Thành phần chính:**
- Route: `/api/coupon`
- Controller: `coupon.controller.js`
- Model: `coupon.model.js`

**Endpoints:**
```
GET    /api/coupon                     - Lấy danh sách coupon (admin)
POST   /api/coupon                     - Tạo coupon (admin)
PUT    /api/coupon/:id                 - Cập nhật coupon (admin)
DELETE /api/coupon/:id                 - Xóa coupon (admin)
GET    /api/coupon/validate/:code      - Kiểm tra coupon hợp lệ
```

---

### 2.15 Utility Subsystems

#### 2.15.1 File Management
- Route: `/api/file`
- Chức năng: Upload, download, xóa file
- Hỗ trợ: Ảnh, tài liệu

#### 2.15.2 Location Management (Provinces)
- Route: `/api/provinces`
- Chức năng: Lấy danh sách tỉnh/thành phố
- Mục đích: Hỗ trợ giao hàng

#### 2.15.3 Chatbot
- Route: `/api/chat-bot`
- Controller: `chatBox.controller.js`
- Chức năng: Bot hỗ trợ khách hàng (AI hoặc FAQ)

---

## 3. Giao Diện Hệ Thống (System Interfaces)

### 3.1 API Interface Layer
**Mô tả:** Tất cả subsystem giao tiếp qua REST API

**Protocol:** HTTP/HTTPS  
**Format Data:** JSON  
**Authentication:** JWT Token + HTTP-only Cookies  
**Rate Limiting:**
- Global: `globalLimiter`
- Auth: `authApiLimiter`
- Login: `loginLimiter`
- Refresh Token: `refreshLimiter`

**Request Header:**
```javascript
{
  "Authorization": "Bearer {token}",
  "Content-Type": "application/json",
  "Accept": "application/json"
}
```

**Response Format:**
```javascript
{
  "status": "success|error",
  "data": { /* data */ },
  "message": "string",
  "statusCode": 200
}
```

---

### 3.2 Database Interface
**Type:** MongoDB  
**Connection:** Mongoose ORM  
**Features:**
- Transactions
- Indexes
- TTL (Time To Live) cho session
- Aggregation Pipeline

**Collections:**
```
users
products
categories
subcategories
brands
materials
gemstones
orders
items
carts
reviews
custom_orders
coupons
comparisons
wishlist
conversations
```

---

### 3.3 Real-time Communication Interface
**Technology:** Socket.io (WebSocket)

**Namespace:** `/` (Default)

**Room Structure:**
- `{userId}` - Room khách hàng cá nhân
- `admins` - Room tất cả admin

**Message Structure:**
```javascript
{
  roomId: string,
  from: "customer|admin",
  message: string,
  isReadByAdmin: boolean,
  timestamp: Date
}
```

---

### 3.4 File Upload Interface
**Method:** Multer  
**Storage:** Disk Storage  
**Paths:**
- Products: `src/uploads/products/`
- Files: `uploads/files/`

**Limits:**
- File size: 10MB
- Product images: 10 files per upload
- Accepted: Images (jpg, png, etc.), Excel files

---

### 3.5 External Service Interface

#### Payment Gateway Interface
- **Provider:** PayOS
- **Methods:** QR code, Bank transfer
- **Endpoints:**
  - Create payment link
  - Callback/Webhook for confirmation

#### Google OAuth Interface
- **Provider:** Google
- **Scope:** Email, Profile
- **Endpoint:** POST `/api/sign-in-google`

#### Location Service Interface
- **Data:** Vietnamese provinces/districts
- **Endpoint:** GET `/api/provinces`

---

### 3.6 Middleware Interface
**Middleware Stack:**

```
Request Flow:
1. Express JSON Parser
2. Cookie Parser
3. CORS Middleware
4. Helmet (Security)
5. Rate Limiter (Global)
6. Route Handler
7. Authentication Check (if needed)
8. Role Check (if needed)
9. Schema Validation
10. Controller Logic
11. Response
```

**Key Middleware:**
- `checkAuth.js` - Xác thực JWT token
- `checkRole.js` - Kiểm tra quyền người dùng (admin/user)
- `middleware.js` - Validation middleware

---

## 4. Data Flow Diagram

### 4.1 Regular Order Flow
```
User 
  ├─> Browse Products (Product Subsystem)
  ├─> Add to Cart (Shopping Cart Subsystem)
  ├─> View Cart → Checkout (Order Processing)
  ├─> Apply Coupon (Coupon Subsystem)
  ├─> Create Order (Order Subsystem)
  ├─> Process Payment (Payment Subsystem)
  ├─> Payment Callback (Order Status Update)
  ├─> Track Order (Order Subsystem)
  └─> Leave Review (Review Subsystem)
```

### 4.2 Custom Order Flow
```
User
  ├─> Start Custom Design (Custom Order Subsystem)
  ├─> Select Materials & Gemstones (Attributes Subsystem)
  ├─> Calculate Price (Custom Subsystem)
  ├─> Preview Design (Custom Subsystem)
  ├─> Place Custom Order (Custom Order Subsystem)
  ├─> Process Payment (Payment Subsystem)
  ├─> Admin Review (Dashboard)
  ├─> Update Status (Order Status)
  └─> Delivery & Review (Review Subsystem)
```

### 4.3 Admin Management Flow
```
Admin
  ├─> Dashboard (Analytics)
  ├─> Manage Products (Product Subsystem)
  ├─> Manage Orders (Order Subsystem)
  ├─> Review Custom Requests (Custom Subsystem)
  ├─> Manage Coupons (Coupon Subsystem)
  ├─> Chat with Customers (Communication Subsystem)
  └─> Manage Categories (Product Attributes)
```

---

## 5. Subsystem Dependencies

```
Mermaid Diagram:
graph TD
    A[Client/Frontend] -->|HTTP/REST| B[API Gateway]
    B --> C[Auth Subsystem]
    B --> D[Product Subsystem]
    B --> E[Cart Subsystem]
    B --> F[Order Processing]
    B --> G[Payment Subsystem]
    B --> H[Communication]
    
    F --> G
    F --> I[Database]
    E --> F
    D --> F
    G --> I
    H --> J[Socket.io]
    
    C -->|JWT Token| B
    D --> I
    E --> I
    F --> I
    H --> I
```

---

## 6. Interface Contracts (Detailed Specifications)

### 6.1 Authentication Interface Contract

**POST /api/sign-up**
```javascript
Request: {
  email: string (required, valid email),
  password: string (required, min 6 chars),
  fullName: string (required)
}

Response: {
  status: "success",
  data: {
    userId: ObjectId,
    email: string,
    token: JWT,
    refreshToken: string
  }
}
```

**POST /api/sign-in**
```javascript
Request: {
  email: string (required),
  password: string (required)
}

Response: {
  status: "success",
  data: {
    userId: ObjectId,
    token: JWT,
    refreshToken: string,
    user: { email, fullName, role }
  }
}
```

---

### 6.2 Product Interface Contract

**GET /api/product**
```javascript
Query: {
  page: number (default: 1),
  limit: number (default: 10),
  category: string (optional),
  sort: string (optional),
  search: string (optional)
}

Response: {
  status: "success",
  data: {
    products: Array[Product],
    pagination: { page, limit, total }
  }
}
```

---

### 6.3 Order Interface Contract

**POST /api/order**
```javascript
Request: {
  items: [
    {
      productId: ObjectId,
      quantity: number,
      price: number
    }
  ],
  shippingAddress: {
    street: string,
    city: string,
    province: string,
    zipCode: string
  },
  couponCode: string (optional),
  paymentMethod: "bank_transfer|card"
}

Response: {
  status: "success",
  data: {
    orderId: ObjectId,
    totalAmount: number,
    paymentLink: string,
    status: "pending"
  }
}
```

---

### 6.4 Custom Order Interface Contract

**POST /api/custom**
```javascript
Request: {
  designDescription: string,
  materials: [ObjectId],
  gemstones: [ObjectId],
  quantity: number,
  notes: string
}

Response: {
  status: "success",
  data: {
    customOrderId: ObjectId,
    estimatedPrice: number,
    status: "pending_design"
  }
}
```

---

## 7. Security Considerations

**Authentication:** JWT Token + Refresh Token Pattern  
**Authorization:** Role-based (Admin, User)  
**Data Validation:** Zod/Joi Schema validation  
**Rate Limiting:** Express Rate Limiter  
**CORS:** Configured for `http://localhost:5173`  
**Helmet:** Security headers  
**Password:** Hashing (bcrypt assumed)  
**Cookies:** HTTP-only, Secure flags

---

## 8. Tóm Tắt Mục Đích Từng Subsystem

| Subsystem | Mục Đích | Người Dùng |
|-----------|----------|-----------|
| Auth | Xác thực & phân quyền | Tất cả |
| Product | Quản lý sản phẩm | Admin, User |
| Custom Order | Đơn hàng tùy chỉnh | User, Admin |
| Cart | Giỏ hàng | User |
| Order | Đơn hàng thông thường | User, Admin |
| Payment | Thanh toán | User, Payment Service |
| Attributes | Danh mục, thương hiệu, vật liệu | Admin, User |
| Compare | So sánh sản phẩm | User |
| Wishlist | Danh sách yêu thích | User |
| Review | Đánh giá sản phẩm | User, Admin |
| Communication | Chat real-time | User, Admin |
| User Management | Thông tin người dùng | User, Admin |
| Dashboard | Thống kê & phân tích | Admin |
| Coupon | Mã giảm giá | User, Admin |
| Utilities | Hỗ trợ chức năng khác | Tất cả |

---

## 9. Kết Luận

Hệ thống Jewelry được thiết kế theo mô hình **Microservices-like Architecture** với 15 subsystem chính, mỗi subsystem có vai trò riêng nhưng giao tiếp thống nhất qua REST API. Kiến trúc này cho phép:

✓ Dễ mở rộng (Scalability)
✓ Dễ bảo trì (Maintainability)
✓ Dễ kiểm thử (Testability)
✓ Tách biệt concern (Separation of Concerns)
✓ Tái sử dụng code (Reusability)

Các giao diện được chuẩn hóa và có hợp đồng rõ ràng, hỗ trợ cả HTTP API và WebSocket cho real-time communication.
