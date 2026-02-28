# HỆ THỐNG QUẢN LÝ BÁN TRANG SỨC - CLASS DIAGRAM

## I. TỔNG QUAN HỆ THỐNG

Hệ thống được xây dựng dựa trên công nghệ:
- **Frontend**: React.js
- **Backend**: Node.js (Express)
- **Database**: MongoDB

---

## II. CẤU TRÚC CLASS DIAGRAM

### 1. USER CLASS (Người dùng)

```
┌─────────────────────────────────────┐
│           USER                      │
├─────────────────────────────────────┤
│ Attributes:                         │
│ - fullName: String                  │
│ - email: String (unique)            │
│ - password: String                  │
│ - phone: String                     │
│ - role: Enum ["user", "admin"]      │
│ - avatar: String (URL)              │
│ - address: String                   │
│ - provider: String (local/oauth)    │
│ - createdAt: Date                   │
│ - updatedAt: Date                   │
├─────────────────────────────────────┤
│ Relations:                          │
│ - 1 User: N Orders                  │
│ - 1 User: 1 Cart                    │
│ - 1 User: 1 Wish List              │
│ - 1 User: N Reviews                 │
│ - 1 User: 1 Compare List           │
│ - 1 User: N Custom Orders          │
│ - 1 User: N Conversations          │
└─────────────────────────────────────┘
```

**Attributes Chi Tiết:**

| Attribute | Type | Required | Unique | Default |
|-----------|------|----------|--------|---------|
| fullName | String | ✓ | ✗ | - |
| email | String | ✓ | ✓ | - |
| password | String | ✗ | ✗ | - |
| phone | String | ✗ | ✗ | - |
| role | String | ✗ | ✗ | "user" |
| avatar | String | ✗ | ✗ | URL mặc định |
| address | String | ✗ | ✗ | - |
| provider | String | ✗ | ✗ | "local" |

---

### 2. PRODUCT CLASS (Sản phẩm)

```
┌──────────────────────────────────────────────┐
│              PRODUCT                         │
├──────────────────────────────────────────────┤
│ Attributes:                                  │
│ - slug: String (unique)                      │
│ - name: String                               │
│ - brandId: ObjectId (ref: Brand)             │
│ - categoryId: ObjectId (ref: Category)       │
│ - subCategoryId: ObjectId (ref: SubCategory)│
│ - variants: Array[ProductVariant]            │
│ - images: Array[ProductImage]                │
│ - description: String                        │
│ - isFeatured: Boolean                        │
│ - isNewProduct: Boolean                      │
│ - rating: Number (0-5)                       │
│ - reviewCount: Number                        │
│ - promotion: PromotionInfo                   │
│ - createdAt: Date                            │
│ - updatedAt: Date                            │
├──────────────────────────────────────────────┤
│ Nested Objects:                              │
│ • ProductVariant:                            │
│   - color: String                            │
│   - options: Array[VariantOption]            │
│                                              │
│ • VariantOption:                             │
│   - sku: String (unique)                     │
│   - itemId: String                           │
│   - type: Enum [CARAT, GRAM, NONE]          │
│   - value: Number                            │
│   - purity: String                           │
│   - originalPrice: Number                    │
│   - finalPrice: Number                       │
│   - stockQuantity: Number                    │
│                                              │
│ • ProductImage:                              │
│   - url: String                              │
│   - isMain: Boolean                          │
│                                              │
│ • PromotionInfo:                             │
│   - discount: Number (0-100%)                │
│   - startAt: Date                            │
│   - endAt: Date                              │
│   - isActive: Boolean                        │
│   - durationHours: Number                    │
│   - durationDays: Number                     │
├──────────────────────────────────────────────┤
│ Relations:                                   │
│ - 1 Brand: N Products                        │
│ - 1 Category: N Products                     │
│ - 1 SubCategory: N Products                  │
│ - 1 Product: N Orders (via OrderItem)       │
│ - 1 Product: N Reviews                       │
│ - 1 Product: N Cart Items                    │
│ - 1 Product: N Wish Items                    │
│ - 1 Product: N Compare Items                 │
└──────────────────────────────────────────────┘
```

---

### 3. ORDER CLASS (Đơn hàng)

```
┌──────────────────────────────────────────────┐
│              ORDER                           │
├──────────────────────────────────────────────┤
│ Attributes:                                  │
│ - orderCode: String (unique)                 │
│ - userId: ObjectId (ref: User)               │
│ - items: Array[OrderItem]                    │
│ - shippingAddress: ShippingAddress           │
│ - subtotal: Number                           │
│ - tax: Number                                │
│ - total: Number                              │
│ - currency: String (default: VND)            │
│ - coupon: ObjectId (ref: Coupon)            │
│ - paymentMethod: Enum [CASH, TRANSFER]      │
│ - paymentStatus: String                      │
│ - status: String                             │
│ - isPaid: Boolean                            │
│ - paidAt: Date                               │
│ - shippedAt: Date                            │
│ - cancelledAt: Date                          │
│ - notes: String                              │
│ - logs: Array[OrderLog]                      │
│ - createdAt: Date                            │
│ - updatedAt: Date                            │
├──────────────────────────────────────────────┤
│ Nested Objects:                              │
│ • OrderItem:                                 │
│   - productId: ObjectId (ref: Product)       │
│   - sku: String                              │
│   - name: String                             │
│   - images: Array[String]                    │
│   - type: Enum [CARAT, GRAM, MM, ...]       │
│   - value: Number                            │
│   - purity: String                           │
│   - unitPrice: Number                        │
│   - quantity: Number (≥ 1)                   │
│   - discount: Number                         │
│   - totalPrice: Number                       │
│                                              │
│ • ShippingAddress:                           │
│   - name: String                             │
│   - phone: String                            │
│   - address: String                          │
│   - city: String                             │
│   - ward: String                             │
│   - country: String (default: VN)            │
│                                              │
│ • OrderLog:                                  │
│   - status: String                           │
│   - by: ObjectId (ref: User)                 │
│   - note: String                             │
│   - at: Date                                 │
├──────────────────────────────────────────────┤
│ Payment Status: PENDING, PAID, FAILED,       │
│                 REFUNDED                     │
│ Order Status: PENDING, CONFIRMED,            │
│              PACKAGING, SHIPPED,             │
│              COMPLETED, CANCELLED,           │
│              REFUNDED                        │
├──────────────────────────────────────────────┤
│ Relations:                                   │
│ - 1 User: N Orders                           │
│ - 1 Order: 1 Coupon (optional)               │
│ - 1 Product: N Orders (via OrderItem)       │
└──────────────────────────────────────────────┘
```

---

### 4. CART CLASS (Giỏ hàng)

```
┌──────────────────────────────────────────────┐
│              CART                            │
├──────────────────────────────────────────────┤
│ Attributes:                                  │
│ - userId: ObjectId (ref: User) [unique]     │
│ - items: Array[CartItem]                     │
│ - createdAt: Date                            │
│ - updatedAt: Date                            │
├──────────────────────────────────────────────┤
│ Nested Objects:                              │
│ • CartItem:                                  │
│   - productId: ObjectId (ref: Product)       │
│   - color: String                            │
│   - sku: String                              │
│   - type: Enum [CARAT, GRAM, MM, ...]       │
│   - value: Number                            │
│   - purity: String                           │
│   - unitPrice: Number                        │
│   - totalPrice: Number                       │
│   - quantity: Number (≥ 1)                   │
│   - stockQuantity: Number                    │
├──────────────────────────────────────────────┤
│ Relations:                                   │
│ - 1 User: 1 Cart                             │
│ - 1 Product: N Cart Items                    │
└──────────────────────────────────────────────┘
```

---

### 5. WISH LIST CLASS (Danh sách yêu thích)

```
┌──────────────────────────────────────────────┐
│              WISH LIST                       │
├──────────────────────────────────────────────┤
│ Attributes:                                  │
│ - userId: ObjectId (ref: User) [unique]     │
│ - items: Array[WishItem]                     │
│ - createdAt: Date                            │
│ - updatedAt: Date                            │
├──────────────────────────────────────────────┤
│ Nested Objects:                              │
│ • WishItem:                                  │
│   - productId: ObjectId (ref: Product)       │
│   - images: Array[ProductImage]              │
│   - price: Number                            │
├──────────────────────────────────────────────┤
│ Relations:                                   │
│ - 1 User: 1 Wish List                        │
│ - 1 Product: N Wish Items                    │
└──────────────────────────────────────────────┘
```

---

### 6. REVIEW CLASS (Đánh giá sản phẩm)

```
┌──────────────────────────────────────────────┐
│              REVIEW                          │
├──────────────────────────────────────────────┤
│ Attributes:                                  │
│ - productId: ObjectId (ref: Product)         │
│ - userId: ObjectId (ref: User)               │
│ - orderItemId: ObjectId (ref: Order)         │
│ - rating: Number (1-5)                       │
│ - comment: String                            │
│ - createdAt: Date                            │
│ - updatedAt: Date                            │
├──────────────────────────────────────────────┤
│ Relations:                                   │
│ - 1 Product: N Reviews                       │
│ - 1 User: N Reviews                          │
└──────────────────────────────────────────────┘
```

---

### 7. CUSTOM ORDER CLASS (Đơn hàng tùy chỉnh)

```
┌──────────────────────────────────────────────┐
│          CUSTOM ORDER                        │
├──────────────────────────────────────────────┤
│ Attributes:                                  │
│ - orderCode: String (unique)                 │
│ - userId: ObjectId (ref: User)               │
│ - jewelryType: String                        │
│ - material: ObjectId (ref: Material)         │
│ - gem: ObjectId (ref: GemStone)              │
│ - size: Number                               │
│ - quantity: Number                           │
│ - gram: Number                               │
│ - carat: Number                              │
│ - subTotal: Number                           │
│ - total: Number                              │
│ - coupon: ObjectId (ref: Coupon)            │
│ - tax: Number                                │
│ - status: String                             │
│ - shippingAddress: ShippingAddress           │
│ - paymentMethod: Enum [TRANSFER]             │
│ - paymentStatus: String                      │
│ - active: String                             │
│ - isPaid: Boolean                            │
│ - paidAt: Date                               │
│ - shippedAt: Date                            │
│ - cancelledAt: Date                          │
│ - notes: String                              │
│ - logs: Array[OrderLog]                      │
│ - createdAt: Date                            │
│ - updatedAt: Date                            │
├──────────────────────────────────────────────┤
│ Status Values:                               │
│ - Status: PENDING, APPROVED, CANCEL          │
│ - Active: PENDING, CONFIRMED, PROCESSING,    │
│           SHIPPED, CANCELLED, REFUNDED       │
│ - Payment Status: PENDING, PAID, FAILED      │
├──────────────────────────────────────────────┤
│ Relations:                                   │
│ - 1 User: N Custom Orders                    │
│ - 1 Material: N Custom Orders                │
│ - 1 GemStone: N Custom Orders                │
│ - 1 Coupon: N Custom Orders (optional)       │
└──────────────────────────────────────────────┘
```

---

### 8. CATEGORY CLASS (Danh mục)

```
┌──────────────────────────────────────────────┐
│              CATEGORY                        │
├──────────────────────────────────────────────┤
│ Attributes:                                  │
│ - name: String                               │
│ - slug: String (unique)                      │
│ - description: String                        │
│ - createdAt: Date                            │
│ - updatedAt: Date                            │
├──────────────────────────────────────────────┤
│ Relations:                                   │
│ - 1 Category: N SubCategories                │
│ - 1 Category: N Products                     │
└──────────────────────────────────────────────┘
```

---

### 9. SUBCATEGORY CLASS (Danh mục con)

```
┌──────────────────────────────────────────────┐
│          SUBCATEGORY                         │
├──────────────────────────────────────────────┤
│ Attributes:                                  │
│ - name: String                               │
│ - slug: String (unique)                      │
│ - categoryId: ObjectId (ref: Category)       │
│ - images: Array[ProductImage]                │
│ - description: String                        │
│ - createdAt: Date                            │
│ - updatedAt: Date                            │
├──────────────────────────────────────────────┤
│ Relations:                                   │
│ - 1 Category: N SubCategories                │
│ - 1 SubCategory: N Products                  │
└──────────────────────────────────────────────┘
```

---

### 10. BRAND CLASS (Thương hiệu)

```
┌──────────────────────────────────────────────┐
│              BRAND                           │
├──────────────────────────────────────────────┤
│ Attributes:                                  │
│ - name: String                               │
│ - slug: String (unique)                      │
│ - createdAt: Date                            │
│ - updatedAt: Date                            │
├──────────────────────────────────────────────┤
│ Relations:                                   │
│ - 1 Brand: N Products                        │
└──────────────────────────────────────────────┘
```

---

### 11. COUPON CLASS (Mã giảm giá)

```
┌──────────────────────────────────────────────┐
│              COUPON                          │
├──────────────────────────────────────────────┤
│ Attributes:                                  │
│ - code: String (unique, uppercase)           │
│ - discountType: Enum [percent, fixed]        │
│ - discountValue: Number                      │
│ - minOrderValue: Number (default: 0)         │
│ - startDate: Date                            │
│ - endDate: Date                              │
│ - isActive: Boolean                          │
│ - createdAt: Date                            │
│ - updatedAt: Date                            │
├──────────────────────────────────────────────┤
│ Relations:                                   │
│ - 1 Coupon: N Orders (optional)              │
│ - 1 Coupon: N Custom Orders (optional)       │
└──────────────────────────────────────────────┘
```

---

### 12. GEMSTONE CLASS (Đá quý)

```
┌──────────────────────────────────────────────┐
│              GEMSTONE                        │
├──────────────────────────────────────────────┤
│ Attributes:                                  │
│ - name: String                               │
│ - slug: String (unique)                      │
│ - type: String (default: GEMSTONE)           │
│ - unit: String (default: CARAT)              │
│ - pricePerUnit: Number                       │
│ - active: Boolean (default: true)            │
│ - createdAt: Date                            │
│ - updatedAt: Date                            │
├──────────────────────────────────────────────┤
│ Relations:                                   │
│ - 1 GemStone: N Custom Orders                │
└──────────────────────────────────────────────┘
```

---

### 13. MATERIAL CLASS (Chất liệu)

```
┌──────────────────────────────────────────────┐
│              MATERIAL                        │
├──────────────────────────────────────────────┤
│ Attributes:                                  │
│ - name: String                               │
│ - slug: String (unique)                      │
│ - type: String (default: METAL)              │
│ - purity: String                             │
│ - unit: String (default: GRAM)               │
│ - pricePerUnit: Number                       │
│ - active: Boolean (default: true)            │
│ - createdAt: Date                            │
│ - updatedAt: Date                            │
├──────────────────────────────────────────────┤
│ Relations:                                   │
│ - 1 Material: N Custom Orders                │
└──────────────────────────────────────────────┘
```

---

### 14. COMPARE CLASS (So sánh sản phẩm)

```
┌──────────────────────────────────────────────┐
│              COMPARE                         │
├──────────────────────────────────────────────┤
│ Attributes:                                  │
│ - userId: ObjectId (ref: User) [unique]     │
│ - items: Array[CompareItem]                  │
│ - createdAt: Date                            │
│ - updatedAt: Date                            │
├──────────────────────────────────────────────┤
│ Nested Objects:                              │
│ • CompareItem:                               │
│   - productId: ObjectId (ref: Product)       │
│   - color: String                            │
│   - sku: String                              │
│   - type: Enum [CARAT, GRAM, MM, NONE]      │
│   - value: Number                            │
│   - purity: String                           │
│   - originalPrice: Number                    │
│   - salePrice: Number                        │
├──────────────────────────────────────────────┤
│ Relations:                                   │
│ - 1 User: 1 Compare                          │
│ - 1 Product: N Compare Items                 │
└──────────────────────────────────────────────┘
```

---

### 15. CONVERSATION CLASS (Hội thoại khách hàng)

```
┌──────────────────────────────────────────────┐
│          CONVERSATION                        │
├──────────────────────────────────────────────┤
│ Attributes:                                  │
│ - roomId: String (unique)                    │
│ - userId: ObjectId (ref: User)               │
│ - messages: Array[Message]                   │
│ - createdAt: Date                            │
│ - updatedAt: Date                            │
├──────────────────────────────────────────────┤
│ Nested Objects:                              │
│ • Message:                                   │
│   - from: Enum [customer, admin]             │
│   - message: String                          │
│   - createdAt: Date                          │
│   - isReadByAdmin: Boolean                   │
├──────────────────────────────────────────────┤
│ Relations:                                   │
│ - 1 User: N Conversations                    │
└──────────────────────────────────────────────┘
```

---

## III. ENTITY RELATIONSHIP DIAGRAM (ERD)

```
                        ┌─────────────┐
                        │    USER     │
                        └──────┬──────┘
                               │
                    ┌──────────┼──────────┐
                    │          │          │
                    │          │          │
        ┌───────────▼────┐  ┌─▼──────┐  ┌▼───────────┐
        │     CART       │  │ ORDER  │  │ WISH LIST  │
        │ (1 per user)   │  │(1: N)  │  │(1 per user)│
        └────────────────┘  └────────┘  └────────────┘
              │                 │              │
              └─────────────────┼──────────────┘
                                │
                           ┌────▼────────┐
                           │  PRODUCT    │
                           │ (1 product: │
                           │  N in cart) │
                           └─────────────┘
                                │
                    ┌───────────┬┼──────────┐
                    │           ││          │
         ┌──────────▼┐  ┌───────▼▼─┐  ┌───▼──────────┐
         │ CATEGORY  │  │SUBCATEGORY│  │   BRAND     │
         └───────────┘  └───────────┘  └─────────────┘


         ┌──────────────┐    ┌──────────────┐
         │  GEMSTONE    │    │   MATERIAL   │
         │ (for customs)│    │ (for customs)│
         └──────┬───────┘    └───────┬──────┘
                │                    │
                └────────────┬───────┘
                             │
                        ┌────▼─────────────┐
                        │  CUSTOM ORDER    │
                        │  (1 user: N)     │
                        └──────────────────┘


         ┌──────────────┐    ┌──────────────┐
         │    COUPON    │    │  CONVERSATION│
         │  (optional)  │    │  (1 user: N) │
         └──────────────┘    └──────────────┘


         ┌──────────────┐    ┌──────────────┐
         │    REVIEW    │    │   COMPARE    │
         │ (1 user: N)  │    │ (1 per user) │
         └──────────────┘    └──────────────┘
```

---

## IV. BẢNG TÓM TẮT QUAN HỆ

| Class A | Quan Hệ | Class B | Ghi Chú |
|---------|---------|---------|----------|
| User | 1:N | Order | Người dùng có nhiều đơn hàng |
| User | 1:1 | Cart | Mỗi người dùng có một giỏ hàng |
| User | 1:1 | Wish List | Mỗi người dùng có một danh sách yêu thích |
| User | 1:N | Review | Người dùng có thể đánh giá nhiều sản phẩm |
| User | 1:1 | Compare | Mỗi người dùng có một danh sách so sánh |
| User | 1:N | Custom Order | Người dùng có nhiều đơn hàng tùy chỉnh |
| User | 1:N | Conversation | Người dùng có nhiều cuộc hội thoại |
| Product | 1:N | Order | Sản phẩm có thể được đặt trong nhiều đơn hàng |
| Product | 1:N | Cart | Sản phẩm có thể được thêm vào giỏ hàng nhiều lần |
| Product | 1:N | Wish | Sản phẩm có thể được yêu thích bởi nhiều người |
| Product | 1:N | Review | Sản phẩm có thể nhận nhiều đánh giá |
| Product | 1:N | Compare | Sản phẩm có thể được so sánh |
| Brand | 1:N | Product | Một thương hiệu có nhiều sản phẩm |
| Category | 1:N | SubCategory | Một danh mục có nhiều danh mục con |
| Category | 1:N | Product | Một danh mục có nhiều sản phẩm |
| SubCategory | 1:N | Product | Một danh mục con có nhiều sản phẩm |
| Material | 1:N | Custom Order | Một chất liệu được dùng cho nhiều đơn hàng tùy chỉnh |
| GemStone | 1:N | Custom Order | Một loại đá quý được dùng cho nhiều đơn hàng tùy chỉnh |
| Coupon | 1:N | Order | Một mã giảm giá có thể được áp dụng cho nhiều đơn hàng |
| Coupon | 1:N | Custom Order | Một mã giảm giá có thể được áp dụng cho nhiều đơn hàng tùy chỉnh |

---

## V. CHỈ SỐ VÀ CÓ LƯỚI (Indexes & Constraints)

### Unique Constraints (Ràng buộc Duy nhất)
- User.email - Email không được trùng lặp
- Product.slug - Slug sản phẩm không được trùng lặp
- Category.slug - Slug danh mục không được trùng lặp
- SubCategory.slug - Slug danh mục con không được trùng lặp
- Brand.slug - Slug thương hiệu không được trùng lặp
- GemStone.slug - Slug đá quý không được trùng lặp
- Material.slug - Slug chất liệu không được trùng lặp
- Coupon.code - Mã coupon không được trùng lặp
- Cart.userId - Mỗi người dùng chỉ có một giỏ hàng
- Wish.userId - Mỗi người dùng chỉ có một danh sách yêu thích
- Compare.userId - Mỗi người dùng chỉ có một danh sách so sánh
- Order.orderCode - Mã đơn hàng không được trùng lặp
- Custom.orderCode - Mã đơn hàng tùy chỉnh không được trùng lặp
- Conversation.roomId - ID phòng không được trùng lặp
- Product.variants[].options[].sku - SKU không được trùng lặp

### Indexed Fields
- Order.orderCode - Tạo index để tìm kiếm nhanh đơn hàng
- Custom.orderCode - Tạo index để tìm kiếm nhanh đơn hàng tùy chỉnh
- Cart.userId - Tìm kiếm giỏ hàng theo người dùng
- Review.productId - Tìm kiếm đánh giá theo sản phẩm

---

## VI. KIỂU DỮ LIỆU CHỦ YẾU

| Kiểu | Mô Tả |
|------|-------|
| String | Chuỗi ký tự |
| Number | Số (nguyên hoặc thập phân) |
| Boolean | Giá trị true/false |
| Date | Ngày giờ |
| ObjectId | Tham chiếu đến document khác |
| Enum | Liệt kê các giá trị cố định |
| Array | Mảng các phần tử |

---

## VII. ENUM VALUES (Các giá trị cố định)

### User Role
- `user` - Người dùng bình thường
- `admin` - Quản trị viên

### Order Payment Status
- `PENDING` - Chờ thanh toán
- `PAID` - Đã thanh toán
- `FAILED` - Thanh toán thất bại
- `REFUNDED` - Đã hoàn tiền

### Order Status
- `PENDING` - Chờ xác nhận
- `CONFIRMED` - Đã xác nhận
- `PACKAGING` - Đang đóng gói
- `SHIPPED` - Đã gửi đi
- `COMPLETED` - Đã hoàn thành
- `CANCELLED` - Đã hủy
- `REFUNDED` - Đã hoàn tiền

### Payment Method
- `CASH` - Thanh toán bằng tiền mặt
- `TRANSFER` - Chuyển khoản

### Custom Order Status
- `PENDING` - Chờ xử lý
- `APPROVED` - Được phê duyệt
- `CANCEL` - Đã hủy

### Discount Type
- `percent` - Giảm giá theo phần trăm
- `fixed` - Giảm giá cố định

### Item Type
- `CARAT` - Tính theo carat (đá quý)
- `GRAM` - Tính theo gram (kim loại)
- `MM` - Tính theo milimét
- `NONE` - Không tính theo đơn vị nào

---

## VIII. CÔNG THỨC TÍNH TOÁN CHÍNH

### Giá sản phẩm trong đơn hàng:
```
OrderItem.totalPrice = OrderItem.unitPrice × OrderItem.quantity
```

### Tổng giá trị đơn hàng:
```
Order.total = Order.subtotal + Order.tax - (Coupon discount if exists)
```

### Tính giảm giá coupon:
```
If Coupon.discountType == "percent":
    discount = Order.subtotal × (Coupon.discountValue / 100)
Else if Coupon.discountType == "fixed":
    discount = Coupon.discountValue
```

### Giá của đơn hàng tùy chỉnh:
```
Custom.subTotal = (Material.pricePerUnit × Custom.gram) + (GemStone.pricePerUnit × Custom.carat)
Custom.total = Custom.subTotal + Custom.tax - (Coupon discount if exists)
```

---

## IX. FLOW CHÍNH CỦA HỆ THỐNG

### Flow Mua Hàng Bình Thường
```
1. User xem sản phẩm (Product)
   ↓
2. User thêm vào giỏ hàng (Cart)
   ↓
3. User review giỏ hàng
   ↓
4. User thanh toán → tạo Order
   ↓
5. Order được xác nhận (Order.status = CONFIRMED)
   ↓
6. Admin đóng gói (Order.status = PACKAGING)
   ↓
7. Order được gửi đi (Order.status = SHIPPED)
   ↓
8. User nhận hàng (Order.status = COMPLETED)
   ↓
9. User review sản phẩm (Review)
```

### Flow Đơn Hàng Tùy Chỉnh
```
1. User chọn loại trang sức (jewelryType)
   ↓
2. User chọn chất liệu (Material)
   ↓
3. User chọn đá quý (GemStone)
   ↓
4. User nhập thông tin chi tiết (size, carat, gram)
   ↓
5. Hệ thống tính giá (Custom.total)
   ↓
6. User thanh toán → tạo Custom Order
   ↓
7. Admin phê duyệt thiết kế (Custom.status = APPROVED)
   ↓
8. Admin xử lý đơn hàng (Custom.active = PROCESSING)
   ↓
9. Order được gửi đi (Custom.active = SHIPPED)
   ↓
10. User nhận hàng (Custom.active = COMPLETED)
```

---

## X. SECURITY & DATA VALIDATION

### Validation Rules

| Field | Rule | Lỗi |
|-------|------|-----|
| User.email | Email format + Unique | Email không hợp lệ hoặc đã tồn tại |
| User.password | Min 6 characters | Password quá ngắn |
| Product.price | > 0 | Giá không được âm |
| Cart quantity | ≥ 1 | Số lượng phải ≥ 1 |
| Review.rating | 1-5 | Rating phải từ 1-5 |
| Coupon.discountValue | > 0 | Giá trị giảm phải > 0 |
| Coupon.minOrderValue | ≥ 0 | Giá trị đơn hàng tối thiểu ≥ 0 |

### Authorization
- Người dùng chỉ có thể xem/sửa dữ liệu của mình
- Admin có quyền xem/sửa tất cả dữ liệu
- Chỉ có admin mới có thể quản lý sản phẩm, danh mục, mã coupon

---

## XI. KẾT LUẬN

Hệ thống gồm 15 entities chính, được tổ chức theo mô hình MongoDB với các references phù hợp. Kiến trúc hỗ trợ đầy đủ các chức năng bán hàng trang sức (bình thường và tùy chỉnh), quản lý người dùng, và hệ thống đánh giá.

**Các đặc điểm chính:**
- ✅ Hỗ trợ 2 loại đơn hàng: thông thường và tùy chỉnh
- ✅ Quản lý giỏ hàng, danh sách yêu thích, so sánh sản phẩm
- ✅ Hệ thống coupon linh hoạt
- ✅ Theo dõi trạng thái đơn hàng chi tiết
- ✅ Hệ thống chat realtime cho khách hàng
- ✅ Đánh giá và xếp hạng sản phẩm

