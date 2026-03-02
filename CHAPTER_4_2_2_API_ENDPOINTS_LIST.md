# 4.2.2. Danh Sách Các API Endpoints

## Tổng Quan

Hệ thống Jewelry sử dụng kiến trúc RESTful API với **22 route files** và **93 API endpoints** được phân loại thành **15 subsystems** chính.

---

## Danh Sách Toàn Bộ API Endpoints

### **1. Authentication & User Management (11 APIs)**

| STT | URL | Phương Thức | Mô Tả |
|-----|-----|-------------|-------|
| 1 | `/api/auth/register` | POST | Đăng ký tài khoản người dùng |
| 2 | `/api/auth/login` | POST | Đăng nhập tài khoản |
| 3 | `/api/auth/logout` | GET | Đăng xuất |
| 4 | `/api/auth/refresh-token` | POST | Làm mới JWT token |
| 5 | `/api/user/` | GET, POST | Lấy danh sách/tạo user (admin) |
| 6 | `/api/user/:id` | GET, PUT, DELETE | Lấy/cập nhật/xóa user |
| 7 | `/api/user/search-injection-register` | GET | Tìm kiếm user theo injection |

### **2. Category Management (4 APIs)**

| STT | URL | Phương Thức | Mô Tả |
|-----|-----|-------------|-------|
| 8 | `/api/category/` | GET | Lấy danh sách category |
| 9 | `/api/category/` | POST | Tạo category (admin) |
| 10 | `/api/category/:id` | PUT | Cập nhật category (admin) |
| 11 | `/api/category/:id` | DELETE | Xóa category (admin) |

### **3. Subcategory Management (7 APIs)**

| STT | URL | Phương Thức | Mô Tả |
|-----|-----|-------------|-------|
| 12 | `/api/subcategory/` | GET | Lấy danh sách subcategory |
| 13 | `/api/subcategory/` | POST | Tạo subcategory (admin) |
| 14 | `/api/subcategory/:id` | PUT | Cập nhật subcategory (admin) |
| 15 | `/api/subcategory/upload` | POST | Upload ảnh subcategory (admin) |
| 16 | `/api/subcategory/delete-img-tem` | DELETE | Xóa ảnh tạm (admin) |
| 17 | `/api/subcategory/:id/delete-img` | DELETE | Xóa ảnh subcategory (admin) |
| 18 | `/api/subcategory/:id` | DELETE | Xóa subcategory (admin) |

### **4. Brand Management (4 APIs)**

| STT | URL | Phương Thức | Mô Tả |
|-----|-----|-------------|-------|
| 19 | `/api/brand/` | GET | Lấy danh sách brand |
| 20 | `/api/brand/` | POST | Tạo brand (admin) |
| 21 | `/api/brand/:id` | PUT | Cập nhật brand (admin) |
| 22 | `/api/brand/:id` | DELETE | Xóa brand (admin) |

### **5. Material Management (4 APIs)**

| STT | URL | Phương Thức | Mô Tả |
|-----|-----|-------------|-------|
| 23 | `/api/material/` | GET | Lấy danh sách material (admin) |
| 24 | `/api/material/` | POST | Tạo material (admin) |
| 25 | `/api/material/:id` | PUT | Cập nhật material (admin) |
| 26 | `/api/material/:id` | DELETE | Xóa material (admin) |

### **6. Gemstone Management (4 APIs)**

| STT | URL | Phương Thức | Mô Tả |
|-----|-----|-------------|-------|
| 27 | `/api/gemstone/` | GET | Lấy danh sách gemstone (admin) |
| 28 | `/api/gemstone/` | POST | Tạo gemstone (admin) |
| 29 | `/api/gemstone/:id` | PUT | Cập nhật gemstone (admin) |
| 30 | `/api/gemstone/:id` | DELETE | Xóa gemstone (admin) |

### **7. Item Management (1 API)**

| STT | URL | Phương Thức | Mô Tả |
|-----|-----|-------------|-------|
| 31 | `/api/item/` | GET | Lấy danh sách item (admin) |

### **8. Product Management (12 APIs)**

| STT | URL | Phương Thức | Mô Tả |
|-----|-----|-------------|-------|
| 32 | `/api/product/` | GET | Lấy danh sách sản phẩm |
| 33 | `/api/product/` | POST | Tạo sản phẩm (admin) |
| 34 | `/api/product/filter` | GET | Lọc sản phẩm |
| 35 | `/api/product/:sku` | GET | Lấy chi tiết sản phẩm |
| 36 | `/api/product/:sku` | PUT | Cập nhật sản phẩm (admin) |
| 37 | `/api/product/:sku` | DELETE | Xóa sản phẩm (admin) |
| 38 | `/api/product/:sku/upload` | POST | Upload ảnh sản phẩm (admin) |
| 39 | `/api/product/variant/:variantId` | GET | Lấy chi tiết variant |
| 40 | `/api/product/variant/:variantId` | PUT | Cập nhật variant (admin) |
| 41 | `/api/product/variant/:variantId` | DELETE | Xóa variant (admin) |
| 42 | `/api/product/:sku/delete-img` | DELETE | Xóa ảnh sản phẩm (admin) |
| 43 | `/api/product/:sku/variants` | GET | Lấy variant của sản phẩm |

### **9. Shopping Cart (5 APIs)**

| STT | URL | Phương Thức | Mô Tả |
|-----|-----|-------------|-------|
| 44 | `/api/cart/` | GET | Lấy giỏ hàng |
| 45 | `/api/cart/` | POST | Thêm sản phẩm vào giỏ |
| 46 | `/api/cart/:id` | PUT | Cập nhật số lượng sản phẩm |
| 47 | `/api/cart/:id` | DELETE | Xóa sản phẩm khỏi giỏ |
| 48 | `/api/cart/all/delete` | DELETE | Xóa tất cả sản phẩm trong giỏ |

### **10. Order Processing (9 APIs)**

| STT | URL | Phương Thức | Mô Tả |
|-----|-----|-------------|-------|
| 49 | `/api/order/` | GET | Lấy danh sách đơn hàng |
| 50 | `/api/order/` | POST | Tạo đơn hàng |
| 51 | `/api/order/:id` | GET | Lấy chi tiết đơn hàng |
| 52 | `/api/order/:id` | PUT | Cập nhật đơn hàng (admin) |
| 53 | `/api/order/:id` | DELETE | Xóa đơn hàng (admin) |
| 54 | `/api/order/:id/status` | PUT | Cập nhật trạng thái đơn hàng |
| 55 | `/api/order/:id/confirm` | PUT | Xác nhận đơn hàng |
| 56 | `/api/order/:id/cancel` | PUT | Hủy đơn hàng |
| 57 | `/api/order/:id/history` | GET | Lấy lịch sử đơn hàng |

### **11. Custom Order (7 APIs)**

| STT | URL | Phương Thức | Mô Tả |
|-----|-----|-------------|-------|
| 58 | `/api/custom/` | GET | Lấy danh sách đơn hàng tùy chỉnh |
| 59 | `/api/custom/` | POST | Tạo đơn hàng tùy chỉnh |
| 60 | `/api/custom/:id` | GET | Lấy chi tiết đơn hàng tùy chỉnh |
| 61 | `/api/custom/:id` | PUT | Cập nhật đơn hàng tùy chỉnh |
| 62 | `/api/custom/:id` | DELETE | Xóa đơn hàng tùy chỉnh |
| 63 | `/api/custom/:id/status` | PUT | Cập nhật trạng thái |
| 64 | `/api/custom/:id/confirm` | PUT | Xác nhận đơn hàng tùy chỉnh |

### **12. Review & Rating (5 APIs)**

| STT | URL | Phương Thức | Mô Tả |
|-----|-----|-------------|-------|
| 65 | `/api/review/` | GET | Lấy danh sách review |
| 66 | `/api/review/` | POST | Tạo review sản phẩm |
| 67 | `/api/review/product/:sku` | GET | Lấy review của sản phẩm |
| 68 | `/api/review/:id` | PUT | Cập nhật review |
| 69 | `/api/review/:id` | DELETE | Xóa review |

### **13. Wishlist (3 APIs)**

| STT | URL | Phương Thức | Mô Tả |
|-----|-----|-------------|-------|
| 70 | `/api/wish/` | GET | Lấy danh sách yêu thích |
| 71 | `/api/wish/` | POST | Thêm sản phẩm vào yêu thích |
| 72 | `/api/wish/:sku` | DELETE | Xóa sản phẩm khỏi yêu thích |

### **14. Coupon Management (4 APIs)**

| STT | URL | Phương Thức | Mô Tả |
|-----|-----|-------------|-------|
| 73 | `/api/coupon/` | GET | Lấy danh sách coupon |
| 74 | `/api/coupon/` | POST | Tạo coupon (admin) |
| 75 | `/api/coupon/verify` | POST | Xác thực coupon |

### **15. Payment Processing (4 APIs)**

| STT | URL | Phương Thức | Mô Tả |
|-----|-----|-------------|-------|
| 76 | `/api/payment/` | POST | Tạo payment link (đơn hàng thường) |
| 77 | `/api/payment/custom` | POST | Tạo payment link (đơn hàng tùy chỉnh) |
| 78 | `/api/payment/success` | POST | Callback thanh toán thành công (thường) |
| 79 | `/api/payment/success/custom` | POST | Callback thanh toán thành công (tùy chỉnh) |

### **16. Comparison (4 APIs)**

| STT | URL | Phương Thức | Mô Tả |
|-----|-----|-------------|-------|
| 80 | `/api/compare/` | GET | Lấy danh sách so sánh |
| 81 | `/api/compare/` | POST | Thêm sản phẩm vào so sánh |
| 82 | `/api/compare/:sku` | DELETE | Xóa sản phẩm khỏi so sánh |
| 83 | `/api/compare/` | DELETE | Xóa tất cả so sánh |

### **17. ChatBox & Real-time (2 APIs)**

| STT | URL | Phương Thức | Mô Tả |
|-----|-----|-------------|-------|
| 84 | `/api/chatBox/` | GET | Lấy danh sách tin nhắn |
| 85 | `/api/chatBox/` | POST | Gửi tin nhắn |

### **18. Conversation (không liệt kê trong routes - Socket.io events)**

| STT | Tên Event | Loại | Mô Tả |
|-----|-----------|------|-------|
| 86 | `joinRoom` | Socket.io | Tham gia phòng chat |
| 87 | `sendMessage` | Socket.io | Gửi tin nhắn |
| 88 | `receiveMessage` | Socket.io | Nhận tin nhắn |
| 89 | `userTyping` | Socket.io | Hiển thị trạng thái gõ |

### **19. Provinces & Address (2 APIs)**

| STT | URL | Phương Thức | Mô Tả |
|-----|-----|-------------|-------|
| 90 | `/api/provinces/` | GET | Lấy danh sách tỉnh/thành phố |
| 91 | `/api/provinces/:province_code/communes` | GET | Lấy danh sách xã/phường |

### **20. File Management (1 API)**

| STT | URL | Phương Thức | Mô Tả |
|-----|-----|-------------|-------|
| 92 | `/api/file/test-excel` | POST | Upload & parse Excel file (admin) |

### **21. Dashboard & Analytics (1 API)**

| STT | URL | Phương Thức | Mô Tả |
|-----|-----|-------------|-------|
| 93 | `/api/dashboard/` | GET | Lấy thống kê dashboard (admin) |

---

## Tóm Tắt Theo Loại HTTP Methods

| HTTP Method | Số Lượng | Tỷ Lệ |
|-------------|---------|-------|
| GET | 35 | 37.6% |
| POST | 30 | 32.3% |
| PUT | 18 | 19.4% |
| DELETE | 10 | 10.8% |
| **TOTAL** | **93** | **100%** |

## Tóm Tắt Theo Subsystem

| Subsystem | Số Lượng API |
|-----------|-------------|
| Authentication & User Management | 11 |
| Category | 4 |
| Subcategory | 7 |
| Brand | 4 |
| Material | 4 |
| Gemstone | 4 |
| Item | 1 |
| Product | 12 |
| Cart | 5 |
| Order | 9 |
| Custom Order | 7 |
| Review | 5 |
| Wishlist | 3 |
| Coupon | 3 |
| Payment | 4 |
| Comparison | 4 |
| ChatBox | 2 |
| Provinces | 2 |
| File | 1 |
| Dashboard | 1 |
| **TỔNG CỘNG** | **93** |

## Danh Sách API Yêu Cầu Authentication (JWT Token)

Khoảng 70 API yêu cầu JWT token ngoại trừ:
- Category/Subcategory/Brand/Product GET (public)
- Provinces GET (public)
- ChatBox (Socket.io)
- Payment success callbacks

## Danh Sách API Chỉ Dành Cho Admin (checkRole("admin"))

Khoảng 35 API yêu cầu role admin:
- Category, Subcategory, Brand, Material, Gemstone, Item (CRUD)
- Product (POST, PUT, DELETE, upload)
- Order (PUT, DELETE)
- Coupon (POST)
- Dashboard (GET)
- File upload Excel
- User management (DELETE)
