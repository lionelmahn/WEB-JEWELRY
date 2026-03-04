# 4.1 Thiết Kế Cơ Sở Dữ Liệu (Database Design)

Hệ thống Jewelry sử dụng MongoDB - một NoSQL database mạnh mẽ với schema linh hoạt. Dữ liệu được lưu trữ dưới dạng JSON-like documents trong các collections.

---

## 4.1.2. Danh Sách Các Collections

Hệ thống Jewelry bao gồm **15 collections** chính được sử dụng để lưu trữ dữ liệu:

| STT | Tên Collection | Ý Nghĩa |
|-----|----------------|---------|
| 1 | User | Lưu thông tin người dùng, tài khoản, quyền hạn |
| 2 | Product | Lưu thông tin sản phẩm trang sức (vàng, bạc, v.v.) |
| 3 | Category | Danh mục sản phẩm cấp 1 (Nhẫn, Dây, Vòng tay, v.v.) |
| 4 | SubCategory | Danh mục sản phẩm cấp 2 (chi tiết hơn Category) |
| 5 | Brand | Thương hiệu, nhà sản xuất trang sức |
| 6 | Material | Lưu thông tin chất liệu (Vàng 18K, Vàng 24K, Bạc, v.v.) |
| 7 | Gemstone | Lưu thông tin đá quý (Kim cương, Ruby, Sapphire, v.v.) |
| 8 | Order | Lưu đơn hàng thường từ khách hàng |
| 9 | Custom | Lưu đơn hàng tùy chỉnh (custom design) từ khách |
| 10 | Cart | Giỏ hàng của từng người dùng |
| 11 | Wish | Danh sách sản phẩm yêu thích của người dùng |
| 12 | Review | Đánh giá và bình luận sản phẩm từ khách hàng |
| 13 | Compare | Danh sách so sánh sản phẩm của người dùng |
| 14 | Coupon | Mã giảm giá, voucher, khuyến mãi |
| 15 | Conversation | Lịch sử chat real-time giữa khách hàng và admin |

---

## 4.1.3. Chi Tiết Các Collections

### 1. Collection: User

**Mô tả**: Lưu thông tin tài khoản, hồ sơ cá nhân, và quyền hạn của người dùng

| STT | Tên Field | Kiểu Dữ Liệu | Mô Tả |
|-----|-----------|-------------|-------|
| 1 | _id | ObjectId (MongoDB) | Mã định danh duy nhất của người dùng |
| 2 | fullName | String | Họ và tên của người dùng (required) |
| 3 | email | String | Email (duy nhất, required) |
| 4 | password | String | Mật khẩu đã mã hóa (bcrypt) |
| 5 | phone | String | Số điện thoại liên lạc |
| 6 | role | String (enum) | Vai trò: "user" hoặc "admin" (mặc định: "user") |
| 7 | avatar | String | URL ảnh đại diện (mặc định: avatar mặc định) |
| 8 | address | String | Địa chỉ nhà của người dùng |
| 9 | provider | String | Nhà cung cấp xác thực: "local" hoặc "google" |
| 10 | createdAt | Date | Thời gian tạo tài khoản (auto) |
| 11 | updatedAt | Date | Thời gian cập nhật lần cuối (auto) |

---

### 2. Collection: Product

**Mô tả**: Lưu thông tin chi tiết về sản phẩm trang sức với các variant, giá, ảnh, và thông tin quản lý

| STT | Tên Field | Kiểu Dữ Liệu | Mô Tả |
|-----|-----------|-------------|-------|
| 1 | _id | ObjectId | Mã định danh sản phẩm |
| 2 | slug | String | URL slug (duy nhất, required) |
| 3 | name | String | Tên sản phẩm (required) |
| 4 | brandId | ObjectId (ref: brand) | Tham chiếu đến Brand |
| 5 | categoryId | ObjectId (ref: category) | Tham chiếu đến Category |
| 6 | subCategoryId | ObjectId (ref: subcategory) | Tham chiếu đến SubCategory |
| 7 | promotion | Object | Khuyến mãi |
| 7.1 | - discount | Number | Phần trăm giảm giá (0-100, mặc định: 0) |
| 7.2 | - startAt | Date | Ngày bắt đầu khuyến mãi |
| 7.3 | - endAt | Date | Ngày kết thúc khuyến mãi |
| 7.4 | - isActive | Boolean | Khuyến mãi có hoạt động không (mặc định: false) |
| 7.5 | - durationHours | Number | Thời lượng khuyến mãi (giờ) |
| 7.6 | - durationDays | Number | Thời lượng khuyến mãi (ngày) |
| 8 | variants | Array | Mảng các variant (phiên bản) sản phẩm |
| 8.1 | - color | String | Màu sắc của variant |
| 8.2 | - options | Array | Mảng các tùy chọn (sku, giá, số lượng) |
| 8.2.1 | -- sku | String | SKU của variant (required) |
| 8.2.2 | -- itemId | String | ID mục hàng (required) |
| 8.2.3 | -- type | String (enum) | Loại: "CARAT", "GRAM", "NONE" |
| 8.2.4 | -- value | Number | Giá trị (carat hoặc gram) |
| 8.2.5 | -- purity | String | Độ tinh khiết (VD: 18K, 24K) |
| 8.2.6 | -- originalPrice | Number | Giá gốc (required) |
| 8.2.7 | -- finalPrice | Number | Giá bán cuối cùng (sau giảm giá) |
| 8.2.8 | -- stockQuantity | Number | Số lượng tồn kho (mặc định: 0) |
| 9 | images | Array | Mảng ảnh sản phẩm |
| 9.1 | - url | String | URL ảnh (required) |
| 9.2 | - isMain | Boolean | Ảnh chính (mặc định: false) |
| 10 | description | String | Mô tả chi tiết sản phẩm |
| 11 | isFeatured | Boolean | Sản phẩm nổi bật (mặc định: false) |
| 12 | isNewProduct | Boolean | Sản phẩm mới (mặc định: false) |
| 13 | rating | Number | Điểm đánh giá trung bình (mặc định: 0) |
| 14 | reviewCount | Number | Số lượng đánh giá (mặc định: 0) |
| 15 | createdAt | Date | Ngày tạo sản phẩm (auto) |
| 16 | updatedAt | Date | Ngày cập nhật (auto) |

---

### 3. Collection: Category

**Mô tả**: Danh mục sản phẩm cấp 1, phân loại chủ yếu (Nhẫn, Dây chuyền, Vòng tay, v.v.)

| STT | Tên Field | Kiểu Dữ Liệu | Mô Tả |
|-----|-----------|-------------|-------|
| 1 | _id | ObjectId | Mã định danh category |
| 2 | name | String | Tên danh mục (required) |
| 3 | slug | String | URL slug (duy nhất, required) |
| 4 | description | String | Mô tả danh mục (required) |
| 5 | createdAt | Date | Ngày tạo (auto) |
| 6 | updatedAt | Date | Ngày cập nhật (auto) |

---

### 4. Collection: SubCategory

**Mô tả**: Danh mục sản phẩm cấp 2, chi tiết hơn (VD: Nhẫn → Nhẫn Kim cương, Nhẫn Vàng)

| STT | Tên Field | Kiểu Dữ Liệu | Mô Tả |
|-----|-----------|-------------|-------|
| 1 | _id | ObjectId | Mã định danh subcategory |
| 2 | name | String | Tên danh mục con (required) |
| 3 | slug | String | URL slug (duy nhất, required) |
| 4 | categoryId | ObjectId (ref: category) | Tham chiếu Category cha |
| 5 | images | Array | Mảng ảnh đại diện |
| 5.1 | - url | String | URL ảnh (required) |
| 5.2 | - isMain | Boolean | Ảnh chính (mặc định: false) |
| 6 | description | String | Mô tả (required) |
| 7 | createdAt | Date | Ngày tạo (auto) |
| 8 | updatedAt | Date | Ngày cập nhật (auto) |

---

### 5. Collection: Brand

**Mô tả**: Thương hiệu, nhà sản xuất trang sức (Tiffany, Cartier, v.v.)

| STT | Tên Field | Kiểu Dữ Liệu | Mô Tả |
|-----|-----------|-------------|-------|
| 1 | _id | ObjectId | Mã định danh brand |
| 2 | name | String | Tên thương hiệu (required) |
| 3 | slug | String | URL slug (duy nhất, required) |
| 4 | createdAt | Date | Ngày tạo (auto) |
| 5 | updatedAt | Date | Ngày cập nhật (auto) |

---

### 6. Collection: Material

**Mô tả**: Thông tin chất liệu kim loại (Vàng 18K, Bạc, Bạch kim, v.v.) với giá / gram

| STT | Tên Field | Kiểu Dữ Liệu | Mô Tả |
|-----|-----------|-------------|-------|
| 1 | _id | ObjectId | Mã định danh material |
| 2 | name | String | Tên chất liệu (required) |
| 3 | slug | String | URL slug (duy nhất, required) |
| 4 | type | String (enum) | Loại: "METAL" (mặc định) |
| 5 | purity | String | Độ tinh khiết (18K, 24K, 925, v.v.) |
| 6 | unit | String (enum) | Đơn vị: "GRAM" (mặc định) |
| 7 | pricePerUnit | Number | Giá mỗi đơn vị (required) |
| 8 | active | Boolean | Hoạt động (mặc định: true) |
| 9 | createdAt | Date | Ngày tạo (auto) |
| 10 | updatedAt | Date | Ngày cập nhật (auto) |

---

### 7. Collection: Gemstone

**Mô tả**: Thông tin đá quý (Kim cương, Ruby, Sapphire, v.v.) với giá / carat

| STT | Tên Field | Kiểu Dữ Liệu | Mô Tả |
|-----|-----------|-------------|-------|
| 1 | _id | ObjectId | Mã định danh gemstone |
| 2 | name | String | Tên đá quý (required) |
| 3 | slug | String | URL slug (duy nhất, required) |
| 4 | type | String (enum) | Loại: "GEMSTONE" (mặc định) |
| 5 | unit | String (enum) | Đơn vị: "CARAT" (mặc định) |
| 6 | pricePerUnit | Number | Giá mỗi carat (required) |
| 7 | active | Boolean | Hoạt động (mặc định: true) |
| 8 | createdAt | Date | Ngày tạo (auto) |
| 9 | updatedAt | Date | Ngày cập nhật (auto) |

---

### 8. Collection: Order

**Mô tả**: Đơn hàng thường từ khách hàng, lưu thông tin sản phẩm, thanh toán, vận chuyển

| STT | Tên Field | Kiểu Dữ Liệu | Mô Tả |
|-----|-----------|-------------|-------|
| 1 | _id | ObjectId | Mã định danh đơn hàng |
| 2 | userId | ObjectId (ref: user) | Tham chiếu người dùng (required) |
| 3 | orderCode | String | Mã đơn hàng (duy nhất, auto-generated) |
| 4 | items | Array | Mảng sản phẩm trong đơn hàng |
| 4.1 | - productId | ObjectId (ref: product) | Tham chiếu sản phẩm |
| 4.2 | - sku | String | SKU sản phẩm |
| 4.3 | - name | String | Tên sản phẩm |
| 4.4 | - images | Array | Ảnh sản phẩm |
| 4.5 | - type | String (enum) | Loại: "CARAT", "GRAM", "MM", "NONE" |
| 4.6 | - value | Number | Giá trị (carat, gram, mm) |
| 4.7 | - purity | String | Độ tinh khiết |
| 4.8 | - unitPrice | Number | Giá / sản phẩm (required) |
| 4.9 | - quantity | Number | Số lượng (min: 1, mặc định: 1) |
| 4.10 | - discount | Number | Chiết khấu cho item |
| 4.11 | - totalPrice | Number | Tổng giá item (required) |
| 5 | shippingAddress | Object | Địa chỉ giao hàng |
| 5.1 | - name | String | Tên người nhận |
| 5.2 | - phone | String | Số điện thoại |
| 5.3 | - address | String | Địa chỉ chi tiết |
| 5.4 | - city | String | Thành phố |
| 5.5 | - ward | String | Phường/Xã |
| 5.6 | - country | String | Quốc gia (mặc định: "VN") |
| 6 | subtotal | Number | Tổng cộng sản phẩm (required) |
| 7 | tax | Number | Thuế (mặc định: 0) |
| 8 | total | Number | Tổng thanh toán (required) |
| 9 | currency | String | Tiền tệ (mặc định: "VND") |
| 10 | coupon | ObjectId (ref: coupon) | Mã giảm giá được áp dụng |
| 11 | paymentMethod | String (enum) | "CASH" hoặc "TRANSFER" (mặc định: "CASH") |
| 12 | paymentStatus | String (enum) | "PENDING", "PAID", "FAILED", "REFUNDED" |
| 13 | status | String (enum) | "PENDING", "CONFIRMED", "PACKAGING", "SHIPPED", "COMPLETED", "CANCELLED", "REFUNDED" |
| 14 | isPaid | Boolean | Đã thanh toán (mặc định: false) |
| 15 | paidAt | Date | Thời gian thanh toán |
| 16 | shippedAt | Date | Thời gian giao hàng |
| 17 | cancelledAt | Date | Thời gian hủy |
| 18 | notes | String | Ghi chú thêm |
| 19 | logs | Array | Lịch sử thay đổi trạng thái |
| 19.1 | - status | String | Trạng thái |
| 19.2 | - by | ObjectId (ref: user) | Người thực hiện |
| 19.3 | - note | String | Ghi chú |
| 19.4 | - at | Date | Thời gian (mặc định: now) |
| 20 | createdAt | Date | Ngày tạo (auto) |
| 21 | updatedAt | Date | Ngày cập nhật (auto) |

---

### 9. Collection: Custom

**Mô tả**: Đơn hàng tùy chỉnh (custom design), cho phép khách đặt trang sức theo yêu cầu

| STT | Tên Field | Kiểu Dữ Liệu | Mô Tả |
|-----|-----------|-------------|-------|
| 1 | _id | ObjectId | Mã định danh đơn tùy chỉnh |
| 2 | userId | ObjectId (ref: user) | Tham chiếu người dùng (required) |
| 3 | orderCode | String | Mã đơn hàng (duy nhất, auto-generated) |
| 4 | jewelryType | String | Loại trang sức (required) |
| 5 | material | ObjectId (ref: material) | Chất liệu chọn (required) |
| 6 | gem | ObjectId (ref: gemstone) | Đá quý chọn (required) |
| 7 | size | Number | Kích thước (required) |
| 8 | quantity | Number | Số lượng |
| 9 | gram | Number | Trọng lượng (gram) (required) |
| 10 | carat | Number | Trọng lượng (carat) (required) |
| 11 | subTotal | Number | Tổng cộng (required) |
| 12 | total | Number | Tổng thanh toán |
| 13 | coupon | ObjectId (ref: coupon) | Mã giảm giá |
| 14 | tax | Number | Thuế (mặc định: 0) |
| 15 | status | String (enum) | "PENDING", "APPROVED", "CANCEL" |
| 16 | shippingAddress | Object | Địa chỉ giao hàng (tương tự Order) |
| 17 | paymentMethod | String (enum) | "TRANSFER" (mặc định) |
| 18 | paymentStatus | String (enum) | "PENDING", "PAID", "FAILED" |
| 19 | active | String (enum) | "PENDING", "CONFIRMED", "PROCESSING", "SHIPPED", "CANCELLED", "REFUNDED" |
| 20 | isPaid | Boolean | Đã thanh toán (mặc định: false) |
| 21 | paidAt | Date | Thời gian thanh toán |
| 22 | shippedAt | Date | Thời gian giao hàng |
| 23 | cancelledAt | Date | Thời gian hủy |
| 24 | notes | String | Ghi chú |
| 25 | logs | Array | Lịch sử thay đổi trạng thái |
| 26 | createdAt | Date | Ngày tạo (auto) |
| 27 | updatedAt | Date | Ngày cập nhật (auto) |

---

### 10. Collection: Cart

**Mô tả**: Giỏ hàng của người dùng, lưu sản phẩm chưa thanh toán

| STT | Tên Field | Kiểu Dữ Liệu | Mô Tả |
|-----|-----------|-------------|-------|
| 1 | _id | ObjectId | Mã định danh giỏ hàng |
| 2 | userId | ObjectId (ref: user) | Tham chiếu người dùng (duy nhất, required) |
| 3 | items | Array | Mảng sản phẩm trong giỏ |
| 3.1 | - productId | ObjectId (ref: product) | Tham chiếu sản phẩm (required) |
| 3.2 | - color | String | Màu sắc |
| 3.3 | - sku | String | SKU |
| 3.4 | - type | String (enum) | Loại: "CARAT", "GRAM", "NONE" |
| 3.5 | - value | Number | Giá trị |
| 3.6 | - purity | String | Độ tinh khiết |
| 3.7 | - unitPrice | Number | Giá / sản phẩm (required) |
| 3.8 | - totalPrice | Number | Tổng giá |
| 3.9 | - quantity | Number | Số lượng (min: 1, mặc định: 1) |
| 3.10 | - stockQuantity | Number | Số lượng tồn kho |
| 4 | createdAt | Date | Ngày tạo (auto) |
| 5 | updatedAt | Date | Ngày cập nhật (auto) |

---

### 11. Collection: Wish

**Mô tả**: Danh sách sản phẩm yêu thích (wishlist) của người dùng

| STT | Tên Field | Kiểu Dữ Liệu | Mô Tả |
|-----|-----------|-------------|-------|
| 1 | _id | ObjectId | Mã định danh wishlist |
| 2 | userId | ObjectId (ref: user) | Tham chiếu người dùng (duy nhất, required) |
| 3 | items | Array | Mảng sản phẩm yêu thích |
| 3.1 | - productId | ObjectId (ref: product) | Tham chiếu sản phẩm (required) |
| 3.2 | - images | Array | Ảnh sản phẩm |
| 3.3 | - price | Number | Giá sản phẩm (required) |
| 4 | createdAt | Date | Ngày tạo (auto) |
| 5 | updatedAt | Date | Ngày cập nhật (auto) |

---

### 12. Collection: Review

**Mô tả**: Đánh giá và bình luận sản phẩm từ khách hàng đã mua

| STT | Tên Field | Kiểu Dữ Liệu | Mô Tả |
|-----|-----------|-------------|-------|
| 1 | _id | ObjectId | Mã định danh review |
| 2 | productId | ObjectId (ref: product) | Tham chiếu sản phẩm (required) |
| 3 | userId | ObjectId (ref: user) | Tham chiếu người dùng (required) |
| 4 | orderItemId | ObjectId (ref: order) | Tham chiếu mục hàng trong đơn (required) |
| 5 | rating | Number | Điểm đánh giá (1-5) (required) |
| 6 | comment | String | Bình luận chi tiết |
| 7 | createdAt | Date | Ngày tạo (auto) |
| 8 | updatedAt | Date | Ngày cập nhật (auto) |

---

### 13. Collection: Compare

**Mô tả**: Danh sách so sánh sản phẩm của người dùng

| STT | Tên Field | Kiểu Dữ Liệu | Mô Tả |
|-----|-----------|-------------|-------|
| 1 | _id | ObjectId | Mã định danh compare list |
| 2 | userId | ObjectId (ref: user) | Tham chiếu người dùng (duy nhất, required) |
| 3 | items | Array | Mảng sản phẩm so sánh |
| 3.1 | - productId | ObjectId (ref: product) | Tham chiếu sản phẩm (required) |
| 3.2 | - color | String | Màu sắc |
| 3.3 | - sku | String | SKU (required) |
| 3.4 | - type | String (enum) | Loại: "CARAT", "GRAM", "MM", "NONE" |
| 3.5 | - value | Number | Giá trị |
| 3.6 | - purity | String | Độ tinh khiết |
| 3.7 | - originalPrice | Number | Giá gốc (required) |
| 3.8 | - salePrice | Number | Giá bán |
| 4 | createdAt | Date | Ngày tạo (auto) |
| 5 | updatedAt | Date | Ngày cập nhật (auto) |

---

### 14. Collection: Coupon

**Mô tả**: Mã giảm giá, voucher, khuyến mãi cho đơn hàng

| STT | Tên Field | Kiểu Dữ Liệu | Mô Tả |
|-----|-----------|-------------|-------|
| 1 | _id | ObjectId | Mã định danh coupon |
| 2 | code | String | Mã coupon (duy nhất, required, uppercase) |
| 3 | discountType | String (enum) | "percent" (%) hoặc "fixed" (số tiền) (required) |
| 4 | discountValue | Number | Giá trị giảm giá (required) |
| 5 | minOrderValue | Number | Giá trị đơn hàng tối thiểu (mặc định: 0) |
| 6 | startDate | Date | Ngày bắt đầu |
| 7 | endDate | Date | Ngày kết thúc |
| 8 | isActive | Boolean | Đang hoạt động (mặc định: false) |
| 9 | createdAt | Date | Ngày tạo (auto) |
| 10 | updatedAt | Date | Ngày cập nhật (auto) |

---

### 15. Collection: Conversation

**Mô tả**: Lịch sử chat real-time giữa khách hàng và admin qua Socket.io

| STT | Tên Field | Kiểu Dữ Liệu | Mô Tả |
|-----|-----------|-------------|-------|
| 1 | _id | ObjectId | Mã định danh conversation |
| 2 | roomId | String | Mã phòng chat (duy nhất, required) |
| 3 | userId | ObjectId (ref: user) | Tham chiếu người dùng (required) |
| 4 | messages | Array | Mảng tin nhắn |
| 4.1 | - from | String (enum) | "customer" hoặc "admin" (required) |
| 4.2 | - message | String | Nội dung tin nhắn (required) |
| 4.3 | - createdAt | Date | Thời gian gửi (mặc định: now) |
| 4.4 | - isReadByAdmin | Boolean | Admin đã đọc (mặc định: false) |
| 5 | createdAt | Date | Ngày tạo (auto) |
| 6 | updatedAt | Date | Ngày cập nhật (auto) |

---

## 4.1.4. Quan Hệ Giữa Các Collections

### Entity-Relationship Diagram (ERD)

```
User (1) ──────────── (N) Order
User (1) ──────────── (N) Custom
User (1) ──────────── (1) Cart
User (1) ──────────── (1) Wish
User (1) ──────────── (1) Compare
User (1) ──────────── (N) Review
User (1) ──────────── (N) Conversation

Product (1) ──────────── (N) Order (items)
Product (1) ──────────── (N) Cart (items)
Product (1) ──────────── (N) Wish (items)
Product (1) ──────────── (N) Compare (items)
Product (1) ──────────── (N) Review

Category (1) ──────────── (N) SubCategory
Category (1) ──────────── (N) Product

SubCategory (1) ──────────── (N) Product

Brand (1) ──────────── (N) Product

Material (1) ──────────── (N) Custom

Gemstone (1) ──────────── (N) Custom

Coupon (1) ──────────── (N) Order
Coupon (1) ──────────── (N) Custom

Order (1) ──────────── (N) Review
```

### Giải Thích Quan Hệ

- **User - Order/Custom**: Một người dùng có nhiều đơn hàng
- **User - Cart/Wish/Compare**: Mỗi người dùng có một giỏ hàng, một danh sách yêu thích, một danh sách so sánh (1-1)
- **Product - Order/Cart/Wish**: Sản phẩm được lưu trong nhiều đơn hàng, giỏ, wishlist
- **Category - SubCategory - Product**: Phân cấp danh mục
- **Material/Gemstone - Custom**: Chất liệu và đá quý được sử dụng trong đơn tùy chỉnh
- **Coupon - Order/Custom**: Mã giảm giá có thể áp dụng cho cả đơn thường và tùy chỉnh

---

## 4.1.5. Indexes & Constraints

### Indexes (Tối ưu truy vấn)

| Collection | Field | Loại | Mục Đích |
|-----------|-------|------|---------|
| User | email | unique | Mỗi email phải duy nhất |
| Product | slug | unique | URL-friendly identifier |
| Product | categoryId | indexed | Tìm sản phẩm theo category |
| Product | brandId | indexed | Tìm sản phẩm theo brand |
| Order | orderCode | unique | Mã đơn hàng duy nhất |
| Order | userId | indexed | Tìm đơn hàng của user |
| Order | createdAt | indexed | Sắp xếp theo ngày tạo |
| Custom | orderCode | unique | Mã đơn tùy chỉnh duy nhất |
| Cart | userId | unique | Mỗi user một giỏ hàng |
| Wish | userId | unique | Mỗi user một wishlist |
| Compare | userId | unique | Mỗi user một compare list |
| Review | productId | indexed | Lấy review của sản phẩm |
| Coupon | code | unique | Mã coupon duy nhất |
| Conversation | roomId | unique | Mã phòng chat duy nhất |

### Constraints (Ràng buộc)

- **User**: Email phải duy nhất, role chỉ là "user" hoặc "admin"
- **Product**: Slug phải duy nhất, giá >= 0, discount 0-100
- **Order**: Status phải là một trong các enum value, quantity >= 1
- **Coupon**: Code phải uppercase, discountType phải là "percent" hoặc "fixed"
- **Cart/Wish/Compare**: userId phải duy nhất (1 user = 1 collection)

---

## 4.1.6. Data Validation Rules

### User Collection
- fullName: String, required, min 2, max 50 characters
- email: String, required, unique, valid email format
- password: bcrypt hashed, min 6 characters (client-side), salt rounds: 10
- phone: String, valid phone format (VN)
- role: enum ["user", "admin"]
- avatar: valid URL

### Product Collection
- name: String, required
- slug: String, required, unique, URL-safe
- promotion.discount: 0 <= value <= 100
- promotion.startAt < promotion.endAt
- variants.options[].originalPrice >= 0
- variants.options[].stockQuantity >= 0
- rating: 0 <= value <= 5

### Order Collection
- items: minimum 1 item required
- items[].quantity >= 1
- subtotal === sum(items[].totalPrice)
- total >= subtotal (after tax)
- paymentStatus: enum ["PENDING", "PAID", "FAILED", "REFUNDED"]
- status: enum ["PENDING", "CONFIRMED", "PACKAGING", "SHIPPED", "COMPLETED", "CANCELLED", "REFUNDED"]

### Coupon Collection
- code: unique, required, uppercase
- discountType: enum ["percent", "fixed"]
- discountValue > 0
- startDate <= endDate (if both provided)
- minOrderValue >= 0

---

## 4.1.7. Backup & Security

### Backup Strategy
- MongoDB Atlas automatic backups (daily)
- Manual export monthly
- Retention: 30 days for continuous backups, 365 days for long-term

### Security Measures
- Password hashing: bcrypt (10 rounds)
- Sensitive fields encrypted: payment info, personal data
- Row-level access control: Users can only see their own data
- Rate limiting: API endpoints to prevent abuse
- Input validation: All fields validated server-side
- SQL Injection prevention: Using Mongoose (no raw queries)

---

## Tóm Tắt

- **15 Collections** lưu trữ toàn bộ dữ liệu hệ thống
- **Relationships**: Sử dụng ObjectId references (denormalization where needed)
- **Constraints**: Unique indexes, enum validation, required fields
- **Performance**: Indexed frequently queried fields (userId, productId, status)
- **Security**: Encrypted sensitive data, access control, input validation
