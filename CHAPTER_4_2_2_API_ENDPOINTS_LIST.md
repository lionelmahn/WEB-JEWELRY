## 4.2.2 Danh Sách Các API Endpoints

### Danh sách tổng hợp các API endpoints được sử dụng trong hệ thống

| STT | URL | Phương Thức | Mô Tả |
|-----|-----|-----------|------|
| | **AUTHENTICATION SUBSYSTEM** | | |
| 1 | /api/auth/sign-up | POST | Đăng ký tài khoản mới |
| 2 | /api/auth/sign-in | POST | Đăng nhập với email/password |
| 3 | /api/auth/sign-in-google | POST | Đăng nhập với Google OAuth |
| 4 | /api/auth/refresh | POST | Làm mới token (refresh token) |
| | **USER MANAGEMENT SUBSYSTEM** | | |
| 5 | /api/users | GET | Lấy danh sách tất cả người dùng (admin) |
| 6 | /api/users/:id | PUT | Cập nhật thông tin người dùng |
| 7 | /api/users/update-role/:id | PUT | Cập nhật vai trò người dùng (admin) |
| 8 | /api/users/logout | GET | Đăng xuất |
| 9 | /api/users/update-password | POST | Cập nhật mật khẩu |
| 10 | /api/users/upload | POST | Upload avatar |
| 11 | /api/users/:id | DELETE | Xoá người dùng (admin) |
| | **PRODUCT MANAGEMENT SUBSYSTEM** | | |
| 12 | /api/products | GET | Lấy danh sách tất cả sản phẩm |
| 13 | /api/products/date-time | GET | Lấy sản phẩm theo thời gian |
| 14 | /api/products/:id | GET | Lấy chi tiết sản phẩm |
| 15 | /api/products/edit/:id | GET | Lấy sản phẩm để chỉnh sửa (admin) |
| 16 | /api/products | POST | Tạo sản phẩm mới (admin) |
| 17 | /api/products/:id | PUT | Cập nhật sản phẩm (admin) |
| 18 | /api/products/upload | POST | Upload hình ảnh sản phẩm (admin) |
| 19 | /api/products/file-excel | POST | Tải lên file Excel sản phẩm (admin) |
| 20 | /api/products/file-excel-preview | POST | Xem trước dữ liệu file Excel (admin) |
| 21 | /api/products/delete-upload | DELETE | Xoá hình ảnh tạm thời (admin) |
| 22 | /api/products/:id | DELETE | Xoá sản phẩm (admin) |
| 23 | /api/products/:id/image | DELETE | Xoá hình ảnh từ sản phẩm (admin) |
| | **SHOPPING CART SUBSYSTEM** | | |
| 24 | /api/cart | GET | Lấy giỏ hàng của người dùng |
| 25 | /api/cart | POST | Tạo giỏ hàng |
| 26 | /api/cart/:sku | PATCH | Cập nhật số lượng sản phẩm trong giỏ |
| 27 | /api/cart/:sku | DELETE | Xoá sản phẩm khỏi giỏ hàng |
| 28 | /api/cart/clear/:sku | DELETE | Xoá tất cả sản phẩm khỏi giỏ hàng |
| | **ORDER PROCESSING SUBSYSTEM** | | |
| 29 | /api/orders/list-order | GET | Lấy danh sách đơn hàng của người dùng |
| 30 | /api/orders | GET | Lấy tất cả đơn hàng (admin) |
| 31 | /api/orders/preview | POST | Xem trước đơn hàng |
| 32 | /api/orders/use-coupon | POST | Áp dụng mã giảm giá |
| 33 | /api/orders | POST | Tạo đơn hàng mới |
| 34 | /api/orders/:id | GET | Lấy chi tiết đơn hàng |
| 35 | /api/orders/:id/status | PUT | Cập nhật trạng thái đơn hàng (admin) |
| 36 | /api/orders/:id/payment-status | PUT | Cập nhật trạng thái thanh toán |
| 37 | /api/orders/:id | DELETE | Huỷ đơn hàng |
| | **CUSTOM ORDER SUBSYSTEM** | | |
| 38 | /api/custom | GET | Lấy danh sách đơn hàng tùy chỉnh (admin) |
| 39 | /api/custom/user | GET | Lấy đơn hàng tùy chỉnh của người dùng |
| 40 | /api/custom | POST | Tạo đơn hàng tùy chỉnh |
| 41 | /api/custom/calculate | POST | Tính giá đơn hàng tùy chỉnh |
| 42 | /api/custom/preview/:id | POST | Xem trước thiết kế tùy chỉnh |
| 43 | /api/custom/update/:id | PUT | Cập nhật đơn hàng tùy chỉnh |
| 44 | /api/custom/:id | PUT | Cập nhật trạng thái tùy chỉnh (admin) |
| | **REVIEW & RATING SUBSYSTEM** | | |
| 45 | /api/reviews/all | GET | Lấy tất cả đánh giá |
| 46 | /api/reviews | GET | Lấy đánh giá theo sản phẩm |
| 47 | /api/reviews | POST | Tạo đánh giá mới |
| 48 | /api/reviews/:id | PUT | Cập nhật đánh giá |
| 49 | /api/reviews/:id | DELETE | Xoá đánh giá |
| | **WISHLIST SUBSYSTEM** | | |
| 50 | /api/wishlist | GET | Lấy danh sách yêu thích |
| 51 | /api/wishlist | POST | Thêm sản phẩm vào yêu thích |
| 52 | /api/wishlist/:id | DELETE | Xoá sản phẩm khỏi yêu thích |
| | **COUPON MANAGEMENT SUBSYSTEM** | | |
| 53 | /api/coupons | GET | Lấy danh sách mã giảm giá |
| 54 | /api/coupons | POST | Tạo mã giảm giá (admin) |
| 55 | /api/coupons/:id | PUT | Cập nhật mã giảm giá (admin) |
| 56 | /api/coupons/:id | DELETE | Xoá mã giảm giá (admin) |
| | **REAL-TIME COMMUNICATION SUBSYSTEM** | | |
| 57 | /api/conversation | GET | Lấy tất cả tin nhắn |
| 58 | /api/conversation/me | GET | Lấy tin nhắn của người dùng |
| 59 | /api/conversation/hasunread | GET | Kiểm tra tin nhắn chưa đọc |
| 60 | /api/conversation/markread | GET | Đánh dấu tin nhắn đã đọc |

### Tóm Tắt Theo Subsystem

| Subsystem | Tổng Số API | GET | POST | PUT | DELETE | PATCH |
|-----------|------------|-----|------|-----|--------|-------|
| Authentication | 4 | 0 | 3 | 0 | 0 | 0 |
| User Management | 7 | 2 | 2 | 2 | 1 | 0 |
| Product Management | 12 | 4 | 4 | 1 | 3 | 0 |
| Shopping Cart | 5 | 1 | 1 | 1 | 2 | 1 |
| Order Processing | 9 | 3 | 3 | 2 | 1 | 0 |
| Custom Order | 7 | 2 | 3 | 2 | 0 | 0 |
| Review & Rating | 5 | 2 | 1 | 1 | 1 | 0 |
| Wishlist | 3 | 1 | 1 | 0 | 1 | 0 |
| Coupon Management | 4 | 1 | 1 | 1 | 1 | 0 |
| Real-time Communication | 4 | 4 | 0 | 0 | 0 | 0 |
| **TỔNG CỘNG** | **60** | **20** | **19** | **10** | **10** | **1** |

### Các API yêu cầu Authentication

Các API sau yêu cầu người dùng phải đăng nhập (có JWT token):
- Tất cả user management endpoints
- Tất cả cart endpoints
- Tất cả custom order endpoints
- GET /api/orders/list-order
- POST /api/orders (tạo đơn hàng)
- POST /api/reviews (tạo đánh giá)
- PUT /api/reviews (cập nhật đánh giá)
- DELETE /api/reviews (xoá đánh giá)
- Tất cả wishlist endpoints
- Tất cả conversation endpoints

### Các API yêu cầu Role Admin

Các API sau yêu cầu quyền Admin:
- GET /api/users (danh sách người dùng)
- PUT /api/users/update-role/:id
- DELETE /api/users/:id
- POST /api/products (tạo sản phẩm)
- PUT /api/products/:id (cập nhật sản phẩm)
- POST /api/products/upload
- POST /api/products/file-excel
- POST /api/products/file-excel-preview
- DELETE /api/products/:id (xoá sản phẩm)
- GET /api/orders (danh sách tất cả đơn hàng)
- PUT /api/orders/:id/status
- GET /api/custom (danh sách tùy chỉnh)
- PUT /api/custom/:id (cập nhật trạng thái)
- POST /api/coupons (tạo coupon)
- PUT /api/coupons/:id (cập nhật coupon)
- DELETE /api/coupons/:id (xoá coupon)
