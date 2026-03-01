# BÁOCÁO: XÁC ĐỊNH CÁC HỆ THỐNG CON VÀ GIAO DIỆN

**Hệ Thống:** Nền tảng Thương mại điện tử Trang sức (Jewelry E-Commerce Platform)  
**Công nghệ:** React.js | Node.js (Express) | MongoDB  
**Loại Kiến trúc:** N-Layer Architecture (4-Tier)  
**Ngày:** 2026

---

## PHẦN 1: TỔNG QUAN KIẾN TRÚC HỆ THỐNG

### 1.1 Kiến Trúc Tổng Quát (Layered Architecture)

```
┌─────────────────────────────────────────────────────────┐
│          PRESENTATION LAYER (Giao diện người dùng)      │
│                    React.js Components                   │
└────────────────────┬────────────────────────────────────┘
                     │
┌────────────────────▼────────────────────────────────────┐
│              API Gateway & Routing Layer                │
│         (Express.js Routes, Rate Limiting)              │
└────────────────────┬────────────────────────────────────┘
                     │
┌────────────────────▼────────────────────────────────────┐
│    APPLICATION & BUSINESS LOGIC LAYER                   │
│  (Controllers, Services, Business Rules, Validation)    │
└────────────────────┬────────────────────────────────────┘
                     │
┌────────────────────▼────────────────────────────────────┐
│     DATA ACCESS LAYER (Persistence)                     │
│  (MongoDB Models, Schemas, Database Queries)            │
└────────────────────┬────────────────────────────────────┘
                     │
┌────────────────────▼────────────────────────────────────┐
│              DATABASE LAYER                             │
│           MongoDB (Document Store)                      │
└─────────────────────────────────────────────────────────┘

Giao diện bổ sung:
├─ WebSocket Layer (Socket.io) - Real-time Communication
├─ Authentication Layer (JWT Tokens)
├─ External Services (Payment Gateway, Google OAuth)
└─ File Storage (Disk-based Upload)
```

**Đặc điểm:**
- Phân tách rõ ràng theo trách nhiệm (Separation of Concerns)
- Mỗi lớp chỉ tương tác với lớp liền kề
- Dễ kiểm thử từng lớp độc lập
- Hỗ trợ mở rộng từng lớp mà không ảnh hưởng lớp khác

---

## PHẦN 2: XÁC ĐỊNH VÀ PHÂN TÍCHcác HỆ THỐNG CON

### Định nghĩa Subsystem
**Subsystem** là một tập hợp các thành phần logic (Models, Controllers, Routes, Services) được tổ chức để thực hiện một chức năng kinh doanh cụ thể. Mỗi subsystem:
- Có một vai trò rõ ràng
- Sở hữu dữ liệu riêng (hoặc chia sẻ theo hợp đồng)
- Cung cấp giao diện (API) để giao tiếp
- Có thể được phát triển và triển khai độc lập

---

### Hệ thống con 1: **SUBSYSTEM QUẢN LÝ XÁC THỰC VÀ PHÂN QUYỀN** (Authentication & Authorization)

#### 1. Tên Subsystem
**Authentication & Authorization Management Subsystem (AAMS)**

#### 2. Chức Năng Chính
- Xác thực danh tính người dùng (Authentication)
- Phân quyền truy cập tài nguyên (Authorization)
- Quản lý phiên làm việc (Session Management)
- Cấp phát và làm mới token

#### 3. Dữ Liệu Xử Lý
**Model:** `User`
**Trường dữ liệu:**
```javascript
{
  _id: ObjectId,
  email: String (unique),
  password: String (hashed),
  fullName: String,
  role: Enum['user', 'admin'],
  phoneNumber: String,
  address: String,
  avatar: String (URL),
  createdAt: Date,
  updatedAt: Date
}
```

#### 4. Giao Diện Cung Cấp (Provided Interfaces)

**API Endpoints:**
```
[POST]   /api/sign-up
  Request:  { email, password, fullName }
  Response: { userId, token, refreshToken }
  
[POST]   /api/sign-in
  Request:  { email, password }
  Response: { userId, token, refreshToken, user }
  
[POST]   /api/sign-in-google
  Request:  { googleToken }
  Response: { userId, token, refreshToken, user }
  
[POST]   /api/refresh
  Request:  { refreshToken }
  Response: { token, refreshToken }
  
[POST]   /api/logout
  Request:  { token }
  Response: { message: "Logout successful" }
```

**Middleware Exports:**
```javascript
- checkAuth(req, res, next)      // Kiểm tra JWT token
- checkRole(role)(req, res, next) // Kiểm tra quyền người dùng
```

#### 5. Giao Diện Yêu Cầu (Required Interfaces)

| Subsystem Yêu Cầu | Giao Diện | Mục Đích |
|------------------|----------|---------|
| Database | MongoDB Mongoose | Lưu trữ thông tin user |
| External Service | Google OAuth 2.0 | Đăng nhập xã hội |
| Utilities | Password Hashing (bcrypt) | Bảo mật mật khẩu |

#### 6. Dependency và Quan Hệ

```
AAMS được sử dụng bởi tất cả subsystem khác qua middleware checkAuth()

Mũi tên hướng vào AAMS:
- Tất cả subsystem → AAMS (Xác thực trước khi truy cập)

AAMS không phụ thuộc vào subsystem khác (ngoại trừ DB)
```

#### 7. Lưu ý Bảo Mật
- JWT Token lưu trong memory + HTTP-only cookies
- Refresh Token có TTL (Time To Live)
- Password hash với bcrypt
- Rate limiting trên login endpoint

---

### Hệ thống con 2: **SUBSYSTEM QUẢN LÝ SẢN PHẨM** (Product Management)

#### 1. Tên Subsystem
**Product Management System (PMS)**

#### 2. Chức Năng Chính
- Quản lý catalog sản phẩm trang sức
- CRUD sản phẩm (admin)
- Tìm kiếm và lọc sản phẩm
- Upload ảnh sản phẩm
- Import sản phẩm từ file Excel
- Quản lý inventory (kho)

#### 3. Dữ Liệu Xử Lý
**Model:** `Product`
**Trường dữ liệu:**
```javascript
{
  _id: ObjectId,
  productName: String,
  description: String,
  price: Number,
  originalPrice: Number,
  quantity: Number,
  sku: String (unique),
  category: ObjectId (ref: Category),
  subcategory: ObjectId (ref: SubCategory),
  brand: ObjectId (ref: Brand),
  materials: [ObjectId] (ref: Material),
  gemstones: [ObjectId] (ref: Gemstone),
  images: [String] (URLs),
  rating: Number (0-5),
  reviewCount: Number,
  isActive: Boolean,
  createdAt: Date,
  updatedAt: Date
}
```

#### 4. Giao Diện Cung Cấp

**API Endpoints:**
```
[GET]    /api/product
  Query:  { page, limit, category, search, sort, brand }
  Response: { products[], pagination }
  
[GET]    /api/product/:id
  Response: { product: Product }
  
[POST]   /api/product
  Auth:   Admin only
  Request: { productName, description, price, ... }
  Response: { productId, message }
  
[PUT]    /api/product/:id
  Auth:   Admin only
  Request: { productName, description, ... }
  Response: { updatedProduct }
  
[DELETE] /api/product/:id
  Auth:   Admin only
  Response: { message }
  
[POST]   /api/product/upload
  Auth:   Admin only
  Request: FormData (files, productId)
  Response: { imageUrls[] }
  
[POST]   /api/product/file-excel
  Auth:   Admin only
  Request: FormData (excelFile)
  Response: { importedCount, message }
```

**Internal Services:**
```javascript
- getProductById(productId)
- getProductsByCategory(categoryId)
- searchProducts(query, filters)
- updateProductInventory(productId, quantity)
- calculateProductPrice(product)
```

#### 5. Giao Diện Yêu Cầu

| Subsystem Yêu Cầu | Giao Diện | Mục Đích |
|------------------|----------|---------|
| AAMS | checkAuth() | Xác thực người dùng |
| Attributes Subsystems | Category, Brand, Material, Gemstone API | Liên kết thuộc tính |
| File Storage | Multer Upload | Lưu ảnh sản phẩm |
| Database | MongoDB | Lưu trữ dữ liệu sản phẩm |

#### 6. Dependency
```
PMS → AAMS (checkAuth)
PMS → Category Subsystem (getCategoryInfo)
PMS → Brand Subsystem (getBrandInfo)
PMS → Material Subsystem
PMS → Gemstone Subsystem
PMS → File Storage (Upload images)
```

---

### Hệ thống con 3: **SUBSYSTEM QUẢN LÝ ĐƠN HÀNG TUỲ CHỈNH** (Custom Order Management)

#### 1. Tên Subsystem
**Custom Order System (COS)**

#### 2. Chức Năng Chính
- Tạo đơn hàng thiết kế riêng
- Tính toán giá dựa trên thiết kế
- Xem trước thiết kế
- Cập nhật yêu cầu thiết kế
- Theo dõi trạng thái thiết kế

#### 3. Dữ Liệu Xử Lý
**Model:** `Custom`
**Trường dữ liệu:**
```javascript
{
  _id: ObjectId,
  userId: ObjectId (ref: User),
  designDescription: String,
  materials: [ObjectId] (ref: Material),
  gemstones: [
    {
      type: ObjectId,
      quantity: Number,
      price: Number
    }
  ],
  quantity: Number,
  estimatedPrice: Number,
  finalPrice: Number,
  previewImage: String,
  status: Enum['pending_design', 'approved', 'rejected', 'in_production', 'ready'],
  adminNotes: String,
  customerNotes: String,
  createdAt: Date,
  updatedAt: Date,
  completionDate: Date
}
```

#### 4. Giao Diện Cung Cấp

**API Endpoints:**
```
[GET]    /api/custom
  Auth:   Admin only
  Response: { customOrders[] }
  
[GET]    /api/custom/user
  Auth:   User required
  Response: { userCustomOrders[] }
  
[POST]   /api/custom
  Auth:   User required
  Request: { designDescription, materials, gemstones, quantity, notes }
  Response: { customOrderId, estimatedPrice }
  
[POST]   /api/custom/calculate
  Request: { materials, gemstones, quantity }
  Response: { estimatedPrice, breakdown }
  
[POST]   /api/custom/preview/:id
  Response: { previewImage, design }
  
[PUT]    /api/custom/:id
  Auth:   User required
  Request: { designDescription, materials, ... }
  Response: { updatedCustomOrder }
  
[PUT]    /api/custom/:id
  Auth:   Admin only
  Request: { status, adminNotes, finalPrice }
  Response: { updatedCustomOrder }
```

**Internal Services:**
```javascript
- calculateCustomPrice(materials, gemstones, quantity)
- generatePreviewImage(design)
- getCustomOrdersByUser(userId)
- updateCustomOrderStatus(customOrderId, newStatus)
- notifyUserStatusChange(customOrderId, status)
```

#### 5. Giao Diện Yêu Cầu

| Subsystem Yêu Cầu | Giao Diện | Mục Đích |
|------------------|----------|---------|
| AAMS | checkAuth() | Xác thực người dùng |
| Material Subsystem | getMaterialPrice() | Tính giá vật liệu |
| Gemstone Subsystem | getGemstonePrice() | Tính giá đá quý |
| Payment Subsystem | createPaymentLink() | Tạo link thanh toán |
| Notification | notifyUser() | Thông báo cho khách hàng |
| Database | MongoDB | Lưu trữ dữ liệu đơn hàng |

#### 6. Dependency
```
COS → AAMS (checkAuth)
COS → Material Subsystem (getPriceInfo)
COS → Gemstone Subsystem (getPriceInfo)
COS → Payment Subsystem (createPayment)
COS → Database
```

---

### Hệ thống con 4: **SUBSYSTEM QUẢN LÝ GIỎ HÀNG** (Shopping Cart)

#### 1. Tên Subsystem
**Shopping Cart System (SCS)**

#### 2. Chức Năng Chính
- Thêm sản phẩm vào giỏ hàng
- Cập nhật số lượng
- Xóa sản phẩm khỏi giỏ
- Xem chi tiết giỏ hàng
- Tính tổng tiền

#### 3. Dữ Liệu Xử Lý
**Model:** `Cart`
**Trường dữ liệu:**
```javascript
{
  _id: ObjectId,
  userId: ObjectId (ref: User),
  items: [
    {
      productId: ObjectId (ref: Product),
      sku: String,
      quantity: Number,
      price: Number,
      addedAt: Date
    }
  ],
  totalPrice: Number,
  itemCount: Number,
  lastModified: Date
}
```

#### 4. Giao Diện Cung Cấp

**API Endpoints:**
```
[GET]    /api/cart
  Auth:   User required
  Response: { items[], totalPrice, itemCount }
  
[POST]   /api/cart
  Auth:   User required
  Request: { productId, quantity, sku }
  Response: { cartItem, totalPrice }
  
[PATCH]  /api/cart/:sku
  Auth:   User required
  Request: { quantity }
  Response: { updatedItem, totalPrice }
  
[DELETE] /api/cart/:sku
  Auth:   User required
  Response: { message, totalPrice }
  
[DELETE] /api/cart/clear/:sku
  Auth:   User required
  Response: { message }
```

**Internal Services:**
```javascript
- addToCart(userId, productId, quantity)
- updateCartItem(userId, sku, quantity)
- removeFromCart(userId, sku)
- getCart(userId)
- calculateCartTotal(items)
- clearCart(userId)
```

#### 5. Giao Diện Yêu Cầu

| Subsystem Yêu Cầu | Giao Diện | Mục Đích |
|------------------|----------|---------|
| AAMS | checkAuth() | Xác thực người dùng |
| PMS | getProductPrice() | Lấy giá sản phẩm |
| Database | MongoDB | Lưu trữ giỏ hàng |

#### 6. Dependency
```
SCS → AAMS (checkAuth)
SCS → PMS (getProductInfo, getPrice)
SCS → Database
```

---

### Hệ thống con 5: **SUBSYSTEM XỬ LÝ ĐƠN HÀNG** (Order Processing)

#### 1. Tên Subsystem
**Order Processing System (OPS)**

#### 2. Chức Năng Chính
- Tạo đơn hàng từ giỏ hàng
- Quản lý trạng thái đơn hàng
- Áp dụng mã giảm giá
- Xem trước đơn hàng
- Theo dõi đơn hàng
- Hủy đơn hàng

#### 3. Dữ Liệu Xử Lý
**Model:** `Order`
**Trường dữ liệu:**
```javascript
{
  _id: ObjectId,
  userId: ObjectId (ref: User),
  orderNumber: String (unique),
  items: [
    {
      productId: ObjectId,
      quantity: Number,
      price: Number,
      subtotal: Number
    }
  ],
  status: Enum['pending', 'confirmed', 'shipped', 'delivered', 'cancelled'],
  paymentStatus: Enum['pending', 'paid', 'failed'],
  shippingAddress: {
    street: String,
    city: String,
    province: String,
    zipCode: String,
    phoneNumber: String
  },
  subtotal: Number,
  discount: Number,
  couponCode: String,
  tax: Number,
  total: Number,
  notes: String,
  paymentMethod: String,
  createdAt: Date,
  updatedAt: Date,
  deliveredAt: Date
}
```

#### 4. Giao Diện Cung Cấp

**API Endpoints:**
```
[GET]    /api/order/list-order
  Auth:   User required
  Response: { orders[], pagination }
  
[GET]    /api/order
  Auth:   Admin only
  Response: { allOrders[], pagination }
  
[GET]    /api/order/:id
  Auth:   User/Admin required
  Response: { order: Order }
  
[POST]   /api/order
  Auth:   User required
  Request: { items, shippingAddress, couponCode }
  Response: { orderId, total, paymentLink }
  
[POST]   /api/order/preview
  Auth:   User required
  Request: { items, couponCode }
  Response: { preview: { items, subtotal, discount, total } }
  
[POST]   /api/order/use-coupon
  Request: { couponCode, totalAmount }
  Response: { discountAmount, newTotal }
  
[PUT]    /api/order/:id/status
  Auth:   Admin only
  Request: { status }
  Response: { updatedOrder }
  
[PUT]    /api/order/:id/payment-status
  Auth:   Admin/System only
  Request: { paymentStatus }
  Response: { updatedOrder }
  
[DELETE] /api/order/:id
  Auth:   User/Admin required
  Request: { cancellationReason }
  Response: { message, refund }
```

**Internal Services:**
```javascript
- createOrder(userId, items, shippingAddress)
- updateOrderStatus(orderId, status)
- applyCoupon(orderId, couponCode)
- calculateOrderTotal(items, couponCode)
- getOrdersByUser(userId)
- validateShippingAddress(address)
- notifyOrderStatusChange(orderId, status)
```

#### 5. Giao Diện Yêu Cầu

| Subsystem Yêu Cầu | Giao Diện | Mục Đích |
|------------------|----------|---------|
| AAMS | checkAuth() | Xác thực người dùng |
| SCS | getCartItems() | Lấy dữ liệu từ giỏ |
| PMS | getProductInfo() | Lấy thông tin sản phẩm |
| Coupon Subsystem | validateCoupon() | Kiểm tra mã giảm giá |
| Payment Subsystem | createPaymentLink() | Tạo link thanh toán |
| Notification | sendOrderConfirmation() | Gửi thông báo |
| Database | MongoDB | Lưu trữ dữ liệu đơn hàng |

#### 6. Dependency
```
OPS → AAMS (checkAuth)
OPS → SCS (getCartItems)
OPS → PMS (getProductInfo)
OPS → Coupon Subsystem (validateCoupon)
OPS → Payment Subsystem (createPaymentLink)
OPS → Database
OPS ← Payment Subsystem (paymentCallback updates OPS)
```

---

### Hệ thống con 6: **SUBSYSTEM THANH TOÁN** (Payment Processing)

#### 1. Tên Subsystem
**Payment Processing System (PPS)**

#### 2. Chức Năng Chính
- Tạo link thanh toán
- Xử lý callback thanh toán
- Cập nhật trạng thái thanh toán
- Hỗ trợ nhiều phương thức thanh toán
- Quản lý lịch sử giao dịch

#### 3. Dữ Liệu Xử Cấp
**Dữ liệu Xử Lý:**
```javascript
Payment Transaction:
{
  transactionId: String,
  orderId: ObjectId (ref: Order),
  amount: Number,
  currency: String,
  paymentMethod: String,
  status: Enum['pending', 'completed', 'failed', 'cancelled'],
  paymentGatewayResponse: Object,
  createdAt: Date,
  completedAt: Date
}
```

#### 4. Giao Diện Cung Cấp

**API Endpoints:**
```
[POST]   /api/payment
  Auth:   User required
  Request: { orderId, amount, paymentMethod }
  Response: { paymentLink, transactionId }
  
[POST]   /api/payment/custom
  Auth:   User required
  Request: { customOrderId, amount }
  Response: { paymentLink, transactionId }
  
[POST]   /api/payment/success
  Auth:   System/Webhook
  Request: { transactionId, orderId, status }
  Response: { message, orderStatus }
  
[POST]   /api/payment/success/custom
  Auth:   System/Webhook
  Request: { transactionId, customOrderId, status }
  Response: { message, customOrderStatus }
```

**Internal Services:**
```javascript
- createPaymentLink(orderId, amount)
- processPaymentCallback(transactionId, status)
- updateOrderPaymentStatus(orderId, status)
- generatePaymentQRCode(amount, orderId)
- validatePaymentSignature(signature)
```

#### 5. Giao Diện Yêu Cầu

| Subsystem Yêu Cầu | Giao Diện | Mục Đích |
|------------------|----------|---------|
| External Service | PayOS Gateway | Tạo link thanh toán |
| OPS | updateOrderPaymentStatus() | Cập nhật trạng thái đơn |
| COS | updateCustomOrderPaymentStatus() | Cập nhật đơn tùy chỉnh |
| Notification | sendPaymentConfirmation() | Thông báo khách hàng |
| Database | MongoDB | Lưu lịch sử giao dịch |

#### 6. Dependency
```
PPS → PayOS (External Service)
PPS → OPS (updateOrderPaymentStatus)
PPS → COS (updateCustomOrderPaymentStatus)
PPS → Database
OPS → PPS (createPaymentLink)
COS → PPS (createPaymentLink)
```

---

### Hệ thống con 7: **SUBSYSTEM THUỘC TÍNH SẢN PHẨM** (Product Attributes)

#### 1. Tên Subsystem
**Product Attributes System (PAS)**

#### 2. Chức Năng Chính
- Quản lý danh mục (Category)
- Quản lý danh mục con (SubCategory)
- Quản lý thương hiệu (Brand)
- Quản lý vật liệu (Material)
- Quản lý đá quý (Gemstone)

#### 3. Dữ Liệu Xử Lý
**Models:** `Category`, `SubCategory`, `Brand`, `Material`, `Gemstone`

**Category Schema:**
```javascript
{
  _id: ObjectId,
  name: String,
  description: String,
  icon: String,
  image: String,
  isActive: Boolean,
  createdAt: Date
}
```

**Material Schema:**
```javascript
{
  _id: ObjectId,
  name: String,
  description: String,
  basePrice: Number,
  density: Number,
  properties: String,
  createdAt: Date
}
```

**Gemstone Schema:**
```javascript
{
  _id: ObjectId,
  name: String,
  color: String,
  price: Number,
  carat: Number,
  clarity: String,
  cut: String,
  certification: String,
  createdAt: Date
}
```

#### 4. Giao Diện Cung Cấp

**API Endpoints (tương tự cho tất cả attributes):**
```
[GET]    /api/{resource}
  Response: { items[] }
  
[GET]    /api/{resource}/:id
  Response: { item }
  
[POST]   /api/{resource}
  Auth:   Admin only
  Request: { name, description, ... }
  Response: { itemId }
  
[PUT]    /api/{resource}/:id
  Auth:   Admin only
  Request: { name, description, ... }
  Response: { updatedItem }
  
[DELETE] /api/{resource}/:id
  Auth:   Admin only
  Response: { message }
```

**Internal Services:**
```javascript
- getCategoryById(categoryId)
- getMaterialPrice(materialId)
- getGemstonePrice(gemstoneId)
- getAttributesByProductId(productId)
```

#### 5. Giao Diện Yêu Cầu

| Subsystem Yêu Cầu | Giao Diện | Mục Đích |
|------------------|----------|---------|
| AAMS | checkAuth() | Xác thực admin |
| PMS | linkToProduct() | Liên kết sản phẩm |
| COS | getPriceInfo() | Tính giá đơn tùy chỉnh |
| Database | MongoDB | Lưu trữ thuộc tính |

#### 6. Dependency
```
PAS → AAMS (checkAuth)
PMS → PAS (getAttributeInfo)
COS → PAS (getMaterialPrice, getGemstonePrice)
Database → PAS
```

---

### Hệ thống con 8: **SUBSYSTEM GIAO TIẾP THỜI GIAN THỰC** (Real-time Communication)

#### 1. Tên Subsystem
**Real-time Communication System (RCS)**

#### 2. Chức Năng Chính
- Chat thời gian thực giữa khách hàng và admin
- Quản lý cuộc trò chuyện
- Thông báo tin nhắn chưa đọc
- Lưu lịch sử trò chuyện

#### 3. Dữ Liệu Xử Lý
**Model:** `Conversation`
**Trường dữ liệu:**
```javascript
{
  _id: ObjectId,
  roomId: String (userId + adminId),
  participants: [ObjectId],
  messages: [
    {
      from: ObjectId (ref: User),
      content: String,
      isReadByAdmin: Boolean,
      timestamp: Date
    }
  ],
  lastMessage: String,
  lastMessageTime: Date,
  isActive: Boolean,
  createdAt: Date
}
```

#### 4. Giao Diện Cung Cấp

**REST API Endpoints:**
```
[GET]    /api/chat
  Auth:   Admin only
  Response: { conversations[] }
  
[GET]    /api/chat/me
  Auth:   User required
  Response: { myConversations[] }
  
[GET]    /api/chat/hasunread
  Auth:   User required
  Response: { unreadCount, hasUnread }
  
[POST]   /api/chat/markread
  Auth:   User required
  Request: { conversationId }
  Response: { message }
```

**WebSocket Events:**
```javascript
Client → Server:
- customer_open_chat(userId)
  Khách hàng bắt đầu cuộc trò chuyện
  
- customer_message({ userId, roomId, message })
  Khách hàng gửi tin nhắn
  
- join_admin()
  Admin tham gia
  
- admin_message({ roomId, message })
  Admin gửi tin nhắn
  
- admin_join_room(roomId)
  Admin tham gia room cụ thể

Server → Client:
- message({ roomId, from, message, timestamp })
  Tin nhắn mới được gửi
  
- admin_message_notify({ roomId, type })
  Thông báo đến admin
  
- customers_list(list)
  Danh sách khách hàng đang chờ
  
- disconnect
  Ngắt kết nối
```

#### 5. Giao Diện Yêu Cầu

| Subsystem Yêu Cầu | Giao Diện | Mục Đích |
|------------------|----------|---------|
| AAMS | checkAuth() | Xác thực người dùng |
| WebSocket | Socket.io | Giao tiếp thời gian thực |
| Database | MongoDB | Lưu lịch sử trò chuyện |
| Notification | notifyAdmin() | Thông báo tin nhắn mới |

#### 6. Dependency
```
RCS ← Client (WebSocket connection)
RCS → AAMS (checkAuth)
RCS → Socket.io (Emit/Listen events)
RCS → Database (Lưu trữ)
Dashboard → RCS (Xem tin nhắn)
```

---

### Hệ thống con 9-15: **Các subsystem phụ khác**

#### Hệ thống con 9: Subsystem So Sánh Sản Phẩm (Comparison)
- **Chức năng:** Cho phép user so sánh 2-3 sản phẩm
- **Model:** `Compare`
- **Endpoints:** GET, POST, DELETE `/api/compare`
- **Phụ thuộc:** PMS, AAMS, Database

#### Hệ thống con 10: Subsystem Danh Sách Yêu Thích (Wishlist)
- **Chức năng:** Lưu sản phẩm yêu thích
- **Model:** `Wish`
- **Endpoints:** GET, POST, DELETE `/api/wish`
- **Phụ thuộc:** PMS, AAMS, Database

#### Hệ thống con 11: Subsystem Đánh Giá & Nhận Xét (Review)
- **Chức năng:** Cho phép user đánh giá sản phẩm
- **Model:** `Review`
- **Endpoints:** GET, POST, PUT, DELETE `/api/review`
- **Phụ thuộc:** PMS, AAMS, OPS, Database

#### Hệ thống con 12: Subsystem Quản Lý Người Dùng (User Management)
- **Chức năng:** Quản lý thông tin profile người dùng
- **Model:** `User`
- **Endpoints:** GET, PUT `/api/users`
- **Phụ thuộc:** AAMS, Database

#### Hệ thống con 13: Subsystem Bảng Điều Khiển & Thống Kê (Dashboard)
- **Chức năng:** Cung cấp số liệu thống kê cho admin
- **Endpoints:** GET `/api/dashboard`
- **Phụ thuộc:** OPS, PMS, PPS, Database

#### Hệ thống con 14: Subsystem Quản Lý Mã Giảm Giá (Coupon Management)
- **Chức năng:** Tạo, quản lý mã giảm giá
- **Model:** `Coupon`
- **Endpoints:** GET, POST, PUT, DELETE `/api/coupon`
- **Phụ thuộc:** AAMS, OPS, Database

#### Hệ thống con 15: Subsystem Tiện Ích (Utilities)
- **File Management:** Upload, download, xóa file
- **Location Service:** Lấy danh sách tỉnh thành
- **Chatbot:** Bot FAQ/AI hỗ trợ

---

## PHẦN 3: GIAO DIỆN HỆ THỐNG (SYSTEM INTERFACES)

### 3.1 Giao Diện API (REST API Interface)

#### Đặc điểm chung:
- **Protocol:** HTTP/HTTPS
- **Format dữ liệu:** JSON
- **Authentication:** JWT Bearer Token
- **Rate Limiting:** Được áp dụng toàn cục

#### Cấu trúc Request:
```javascript
{
  method: "GET|POST|PUT|DELETE|PATCH",
  url: "/api/{subsystem}/{resource}",
  headers: {
    "Authorization": "Bearer {jwtToken}",
    "Content-Type": "application/json",
    "Accept": "application/json"
  },
  body: { /* payload */ }
}
```

#### Cấu trúc Response:
```javascript
{
  status: "success" | "error",
  statusCode: number,
  data: { /* response data */ } | null,
  message: string,
  timestamp: ISO8601
}
```

**HTTP Status Codes được sử dụng:**
- 200: OK (Thành công)
- 201: Created (Tạo mới thành công)
- 400: Bad Request (Yêu cầu không hợp lệ)
- 401: Unauthorized (Chưa xác thực)
- 403: Forbidden (Không có quyền)
- 404: Not Found (Không tìm thấy)
- 409: Conflict (Xung đột, VD: trùng email)
- 429: Too Many Requests (Quá nhiều yêu cầu)
- 500: Internal Server Error (Lỗi máy chủ)

#### Middleware Pipeline:
```
Request
  ↓
1. JSON Parser
  ↓
2. Cookie Parser
  ↓
3. CORS Validator
  ↓
4. Helmet (Security Headers)
  ↓
5. Global Rate Limiter
  ↓
6. Route Handler
  ↓
7. checkAuth() Middleware (nếu cần)
  ↓
8. checkRole() Middleware (nếu cần)
  ↓
9. Schema Validation (Zod/Joi)
  ↓
10. Controller (Business Logic)
  ↓
11. Response Handler
  ↓
Response
```

---

### 3.2 Giao Diện Database (MongoDB Interface)

#### Đặc điểm:
- **ORM:** Mongoose
- **Database:** MongoDB (Document-based)
- **Connection:** URI string từ environment
- **Features:** Transactions, Indexes, Aggregation Pipeline

#### Các Collections chính:
```
users              → Thông tin người dùng
products           → Sản phẩm có sẵn
categories         → Danh mục
subcategories      → Danh mục con
brands             → Thương hiệu
materials          → Vật liệu
gemstones          → Đá quý
orders             → Đơn hàng
carts              → Giỏ hàng
reviews            → Đánh giá
custom_orders      → Đơn hàng tùy chỉnh
coupons            → Mã giảm giá
comparisons        → So sánh sản phẩm
wishlist           → Danh sách yêu thích
conversations      → Cuộc trò chuyện
```

#### Query Interface:
```javascript
// Create
await Model.create({ ... })

// Read
await Model.findById(id)
await Model.find({ filter })
await Model.findOne({ filter })

// Update
await Model.findByIdAndUpdate(id, { ... }, { new: true })
await Model.updateMany({ ... }, { ... })

// Delete
await Model.findByIdAndDelete(id)

// Aggregation
await Model.aggregate([ { $match: {} }, { $group: {} } ])
```

---

### 3.3 Giao Diện WebSocket (Real-time Communication)

#### Socket.io Configuration:
```javascript
- Namespace: "/" (default)
- Transports: ["websocket", "polling"]
- CORS: Enabled for frontend origin
- Rooms: Dynamic rooms per userId, adminId
```

#### Event Flow:
```
User Browser
  ↓ (emit event)
Socket.io Server
  ↓ (listen)
Handle Handler
  ↓ (broadcast/emit)
Target Client
```

**Các Room:**
- `{userId}` - Room cá nhân của user
- `admins` - Room broadcast đến admin
- `{roomId}` - Room cuộc trò chuyện (userId + adminId)

---

### 3.4 Giao Diện File Storage (Upload/Download)

#### Multer Configuration:
```javascript
- Storage: Disk-based
- Destination: src/uploads/
- File size limit: 10 MB
- Allowed types: .jpg, .png, .gif, .pdf, .xlsx
```

#### Upload Endpoints:
- **Product Images:** `/api/product/upload`
- **Excel Import:** `/api/product/file-excel`
- **Avatar:** `/api/users/avatar`

#### Response:
```javascript
{
  status: "success",
  data: {
    fileUrl: "http://localhost:3000/uploads/...",
    filename: string,
    size: number
  }
}
```

---

### 3.5 Giao Diện Dịch Vụ Ngoài (External Services)

#### Payment Gateway (PayOS):
```javascript
Interface:
- createPaymentLink(amount, orderId, description)
  → Returns: paymentUrl, transactionId
  
- verifyPayment(transactionId)
  → Returns: status, transactionDetails
  
- Webhook Callback: POST /api/payment/success
```

#### Google OAuth:
```javascript
Interface:
- POST /api/sign-in-google
  Request: { token: googleIdToken }
  Response: { userId, token, refreshToken }
```

#### Email Service (nếu có):
```javascript
- sendOrderConfirmation(userId, orderDetails)
- sendPaymentReceipt(userId, transactionId)
- sendStatusUpdate(userId, orderId, status)
```

---

## PHẦN 4: QUAN HỆ PHỤ THUỘC GIỮA SUBSYSTEM

### 4.1 Dependency Matrix (Ma Trận Phụ Thuộc)

|  | AAMS | PMS | COS | SCS | OPS | PPS | PAS | RCS | Other |
|---|------|------|------|------|------|------|------|------|-------|
| AAMS | - | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |
| PMS | - | - | ✓ | - | ✓ | - | ✓ | - | ✓ |
| COS | - | - | - | - | - | ✓ | ✓ | ✓ | ✓ |
| SCS | - | ✓ | - | - | ✓ | - | - | - | - |
| OPS | - | ✓ | - | - | - | ✓ | ✓ | ✓ | ✓ |
| PPS | - | - | - | - | - | - | - | - | ✓ |
| PAS | - | - | - | - | - | - | - | - | - |
| RCS | - | - | - | - | - | - | - | - | - |

**Legend:** ✓ = Phụ thuộc vào

### 4.2 Dependency Graph (Biểu đồ Phụ Thuộc)

```
Database
  ↑
  ├─── AAMS ─────┐
  │              │
  │              ↓
  ├─── PAS ← PMS → OPS ── Notification
  │    ↑         │  ↑
  │    │         │  │
  │    └────────┬┘  │
  │             │   │
  ├─── PMS ─────┘   │
  │    ↑            │
  │    │         ↓  │
  ├─── SCS ──→ OPS  │
  │                 │
  ├─── COS ────→ PPS ← PayOS
  │              │
  │              ↓
  ├─── Coupon ─→ OPS
  │
  ├─── RCS (Socket.io)
  │
  ├─── Comparison
  │
  ├─── Wishlist
  │
  ├─── Review
  │
  ├─── User Management
  │
  ├─── Dashboard (Analytics)
  │
  └─── Utilities (File, Location, Chatbot)
```

### 4.3 Call Chain Chính

#### Regular Order Flow:
```
User
  → PMS (Browse Products)
  → SCS (Add to Cart)
  → AAMS (checkAuth)
  → OPS (Create Order)
  → Coupon (Validate Coupon)
  → PPS (Create Payment Link)
  → PayOS (Process Payment)
  → OPS (Update Order Status)
  → Notification (Send Confirmation)
  → Database (Persist All Data)
```

#### Custom Order Flow:
```
User
  → AAMS (checkAuth)
  → COS (Create Custom Order)
  → PAS (Get Material/Gemstone Prices)
  → COS (Calculate Price)
  → COS (Generate Preview)
  → PPS (Create Payment Link)
  → PayOS (Process Payment)
  → COS (Update Status)
  → Database (Persist All Data)
```

---

## PHẦN 5: KẾT LUẬN VÀ ĐÁNH GIÁ KIẾN TRÚC

### 5.1 Lợi Điểm của Kiến Trúc Đề Xuất

**✓ Separation of Concerns (Tách biệt trách nhiệm)**
- Mỗi subsystem có vai trò rõ ràng
- Thay đổi trong một subsystem ít ảnh hưởng đến subsystem khác

**✓ Scalability (Khả năng mở rộng)**
- Có thể mở rộng từng subsystem độc lập
- Dễ thêm subsystem mới mà không làm gián đoạn hệ thống hiện tại

**✓ Maintainability (Dễ bảo trì)**
- Code được tổ chức theo module logic
- Dễ tìm kiếm và sửa lỗi
- Hỗ trợ team development

**✓ Testability (Dễ kiểm thử)**
- Mỗi subsystem có thể kiểm thử độc lập
- Mock interface từ subsystem khác

**✓ Reusability (Tái sử dụng)**
- Interface rõ ràng cho phép tái sử dụng subsystem
- Dễ mở rộng công năng

### 5.2 Các Thách Thức & Hạn Chế

**! Complexity (Độ phức tạp)**
- Hệ thống có nhiều subsystem → khó quản lý nếu không có documentation tốt

**! Performance Overhead**
- Nhiều lớp request/response → overhead
- Cần optimization ở database query level

**! Consistency (Tính nhất quán)**
- Dữ liệu được chia sẻ giữa các subsystem → khó đảm bảo consistency
- Cần transaction hoặc distributed lock mechanism

### 5.3 Khuyến Nghị

1. **Documentation:** Duy trì API documentation chi tiết (Swagger/OpenAPI)
2. **Testing:** Viết unit test cho từng subsystem
3. **Monitoring:** Đặt logging để theo dõi cross-subsystem interaction
4. **Performance:** Optimize database queries, implement caching (Redis)
5. **Security:** Review lại authentication/authorization logic thường xuyên

---

**END OF DOCUMENT**
