# 4.2.3. Thiết Kế Chi Tiết API

## Tổng Quan

Phần này trình bày chi tiết thiết kế của tất cả 93 API endpoints của hệ thống Jewelry, được tổ chức theo 20 subsystems chính. Mỗi API endpoint được mô tả với các thông tin:
- **URL & Phương thức HTTP**
- **Mục đích & Chức năng**
- **Request (Body/Header/Params)**
- **Response & Status Codes**
- **Validation Rules & Constraints**

---

## 1. AUTHENTICATION SUBSYSTEM (4 APIs)

| STT | URL | Mô Tả Chi Tiết |
|-----|-----|---|
| 1 | `/api/auth/register` | **POST - Đăng ký tài khoản**<br/>- Mục đích: Tạo tài khoản người dùng mới<br/>- Request Body: `{ phoneNumber, identification }`<br/>- Response: `{ success, message, userId, token }`<br/>- Status: 201 Created / 400 Bad Request / 409 Conflict |
| 2 | `/api/auth/login` | **POST - Đăng nhập**<br/>- Mục đích: Xác thực và cấp phát JWT token<br/>- Request Body: `{ phoneNumber, password }`<br/>- Response: `{ success, accessToken, refreshToken, userId, role }`<br/>- Status: 200 OK / 401 Unauthorized / 404 Not Found |
| 3 | `/api/auth/logout` | **GET - Đăng xuất**<br/>- Mục đích: Kết thúc phiên đăng nhập<br/>- Header: `Authorization: Bearer {token}`<br/>- Response: `{ success, message }`<br/>- Status: 200 OK / 401 Unauthorized |
| 4 | `/api/auth/refresh-token` | **POST - Làm mới Token**<br/>- Mục đích: Cấp phát JWT token mới<br/>- Header: `Authorization: Bearer {refreshToken}`<br/>- Response: `{ accessToken, refreshToken }`<br/>- Status: 200 OK / 401 Unauthorized |

## 2. USER MANAGEMENT SUBSYSTEM (7 APIs)

| STT | URL | Mô Tả Chi Tiết |
|-----|-----|---|
| 5 | `/api/user/` | **GET/POST - Danh sách & Tạo User**<br/>- GET: Lấy danh sách user (admin)<br/>- POST: Tạo user mới (admin)<br/>- Response: `{ users: Array, total: Number }`<br/>- Status: 200 OK / 403 Forbidden |
| 6 | `/api/user/:id` | **GET/PUT/DELETE - User Chi Tiết**<br/>- GET: Lấy thông tin user<br/>- PUT: Cập nhật profile<br/>- DELETE: Xóa user (admin)<br/>- Status: 200 OK / 404 Not Found / 403 Forbidden |
| 7 | `/api/user/search-injection-register` | **GET - Tìm kiếm User**<br/>- Mục đích: Tìm kiếm user theo injection<br/>- Status: 200 OK / 400 Bad Request |

## 3. CATEGORY MANAGEMENT (4 APIs)

| STT | URL | Mô Tả Chi Tiết |
|-----|-----|---|
| 8 | `/api/category/` | **GET - Danh sách Category**<br/>- Mục đích: Lấy danh sách danh mục sản phẩm<br/>- Response: `{ categories: Array }`<br/>- Status: 200 OK |
| 9 | `/api/category/` | **POST - Tạo Category (Admin)**<br/>- Request Body: `{ name, slug, description }`<br/>- Header: `checkRole("admin")`<br/>- Status: 201 Created / 403 Forbidden |
| 10 | `/api/category/:id` | **PUT - Cập nhật Category (Admin)**<br/>- Request Body: `{ name, slug, description }`<br/>- Status: 200 OK / 403 Forbidden / 404 Not Found |
| 11 | `/api/category/:id` | **DELETE - Xóa Category (Admin)**<br/>- Status: 200 OK / 403 Forbidden / 404 Not Found |

## 4. SUBCATEGORY MANAGEMENT (7 APIs)

| STT | URL | Mô Tả Chi Tiết |
|-----|-----|---|
| 12 | `/api/subcategory/` | **GET - Danh sách Subcategory**<br/>- Response: `{ subcategories: Array }`<br/>- Status: 200 OK |
| 13 | `/api/subcategory/` | **POST - Tạo Subcategory (Admin)**<br/>- Request Body: `{ name, categoryId, image }`<br/>- Upload: Multer 5 files max<br/>- Status: 201 Created / 403 Forbidden |
| 14 | `/api/subcategory/:id` | **PUT - Cập nhật Subcategory (Admin)**<br/>- Status: 200 OK / 403 Forbidden |
| 15 | `/api/subcategory/upload` | **POST - Upload Ảnh (Admin)**<br/>- Form: `subcatgory-images[]` (max 5 files)<br/>- Status: 200 OK / 400 Bad Request |
| 16 | `/api/subcategory/delete-img-tem` | **DELETE - Xóa Ảnh Tạm (Admin)**<br/>- Status: 200 OK |
| 17 | `/api/subcategory/:id/delete-img` | **DELETE - Xóa Ảnh (Admin)**<br/>- Status: 200 OK / 404 Not Found |
| 18 | `/api/subcategory/:id` | **DELETE - Xóa Subcategory (Admin)**<br/>- Status: 200 OK / 403 Forbidden |

## 5. BRAND MANAGEMENT (4 APIs)

| STT | URL | Mô Tả Chi Tiết |
|-----|-----|---|
| 19 | `/api/brand/` | **GET - Danh sách Brand**<br/>- Status: 200 OK |
| 20 | `/api/brand/` | **POST - Tạo Brand (Admin)**<br/>- Request Body: `{ name, slug }`<br/>- Status: 201 Created / 403 Forbidden |
| 21 | `/api/brand/:id` | **PUT - Cập nhật Brand (Admin)**<br/>- Status: 200 OK / 403 Forbidden |
| 22 | `/api/brand/:id` | **DELETE - Xóa Brand (Admin)**<br/>- Status: 200 OK / 403 Forbidden |

## 6. MATERIAL MANAGEMENT (4 APIs)

| STT | URL | Mô Tả Chi Tiết |
|-----|-----|---|
| 23 | `/api/material/` | **GET - Danh sách Material (Admin)**<br/>- Requires: `checkRole("admin")`<br/>- Status: 200 OK / 403 Forbidden |
| 24 | `/api/material/` | **POST - Tạo Material (Admin)**<br/>- Request Body: `{ name, pricePerGram }`<br/>- Status: 201 Created / 403 Forbidden |
| 25 | `/api/material/:id` | **PUT - Cập nhật Material (Admin)**<br/>- Status: 200 OK / 403 Forbidden |
| 26 | `/api/material/:id` | **DELETE - Xóa Material (Admin)**<br/>- Status: 200 OK / 403 Forbidden |

## 7. GEMSTONE MANAGEMENT (4 APIs)

| STT | URL | Mô Tả Chi Tiết |
|-----|-----|---|
| 27 | `/api/gemstone/` | **GET - Danh sách Gemstone (Admin)**<br/>- Requires: `checkRole("admin")`<br/>- Status: 200 OK / 403 Forbidden |
| 28 | `/api/gemstone/` | **POST - Tạo Gemstone (Admin)**<br/>- Request Body: `{ name, pricePerCarat, color, clarity }`<br/>- Status: 201 Created / 403 Forbidden |
| 29 | `/api/gemstone/:id` | **PUT - Cập nhật Gemstone (Admin)**<br/>- Status: 200 OK / 403 Forbidden |
| 30 | `/api/gemstone/:id` | **DELETE - Xóa Gemstone (Admin)**<br/>- Status: 200 OK / 403 Forbidden |

## 8. ITEM MANAGEMENT (1 API)

| STT | URL | Mô Tả Chi Tiết |
|-----|-----|---|
| 31 | `/api/item/` | **GET - Danh sách Item (Admin)**<br/>- Requires: `checkRole("admin")`<br/>- Response: `{ items: Array }`<br/>- Status: 200 OK / 403 Forbidden |

## 9. PRODUCT MANAGEMENT (12 APIs)

| STT | URL | Mô Tả Chi Tiết |
|-----|-----|---|
| 32 | `/api/product/` | **GET - Danh sách Sản phẩm**<br/>- Query: `{ page, limit, search, sortBy }`<br/>- Response: `{ products: Array, total, page }`<br/>- Status: 200 OK |
| 33 | `/api/product/` | **POST - Tạo Sản phẩm (Admin)**<br/>- Request Body: `{ name, sku, price, categoryId, brandId, description, images, variants }`<br/>- Status: 201 Created / 403 Forbidden |
| 34 | `/api/product/filter` | **GET - Lọc Sản phẩm**<br/>- Query: `{ category, brand, priceMin, priceMax, rating }`<br/>- Status: 200 OK |
| 35 | `/api/product/:sku` | **GET - Chi tiết Sản phẩm**<br/>- Response: `{ product: Object }`<br/>- Status: 200 OK / 404 Not Found |
| 36 | `/api/product/:sku` | **PUT - Cập nhật Sản phẩm (Admin)**<br/>- Request Body: (như create, tất cả optional)<br/>- Status: 200 OK / 403 Forbidden / 404 Not Found |
| 37 | `/api/product/:sku` | **DELETE - Xóa Sản phẩm (Admin)**<br/>- Status: 200 OK / 403 Forbidden |
| 38 | `/api/product/:sku/upload` | **POST - Upload Ảnh (Admin)**<br/>- Form: multipart/form-data<br/>- Status: 200 OK / 400 Bad Request |
| 39 | `/api/product/variant/:variantId` | **GET - Chi tiết Variant**<br/>- Status: 200 OK / 404 Not Found |
| 40 | `/api/product/variant/:variantId` | **PUT - Cập nhật Variant (Admin)**<br/>- Status: 200 OK / 403 Forbidden |
| 41 | `/api/product/variant/:variantId` | **DELETE - Xóa Variant (Admin)**<br/>- Status: 200 OK / 403 Forbidden |
| 42 | `/api/product/:sku/delete-img` | **DELETE - Xóa Ảnh (Admin)**<br/>- Status: 200 OK / 404 Not Found |
| 43 | `/api/product/:sku/variants` | **GET - Danh sách Variant**<br/>- Response: `{ variants: Array }`<br/>- Status: 200 OK |

## 10. SHOPPING CART (5 APIs)

| STT | URL | Mô Tả Chi Tiết |
|-----|-----|---|
| 44 | `/api/cart/` | **GET - Lấy Giỏ hàng**<br/>- Header: `Authorization: Bearer {token}`<br/>- Response: `{ items: Array, total, quantity }`<br/>- Status: 200 OK / 401 Unauthorized |
| 45 | `/api/cart/` | **POST - Thêm vào Giỏ**<br/>- Request Body: `{ variantId, quantity, price }`<br/>- Status: 201 Created / 400 Bad Request |
| 46 | `/api/cart/:id` | **PUT - Cập nhật Số lượng**<br/>- Request Body: `{ quantity }`<br/>- Status: 200 OK / 404 Not Found |
| 47 | `/api/cart/:id` | **DELETE - Xóa khỏi Giỏ**<br/>- Status: 200 OK / 404 Not Found |
| 48 | `/api/cart/all/delete` | **DELETE - Xóa Toàn bộ Giỏ**<br/>- Status: 200 OK |

## 11. ORDER PROCESSING (9 APIs)

| STT | URL | Mô Tả Chi Tiết |
|-----|-----|---|
| 49 | `/api/order/` | **GET - Danh sách Đơn hàng**<br/>- Query: `{ status, page, limit }`<br/>- Response: `{ orders: Array, total }`<br/>- Status: 200 OK |
| 50 | `/api/order/` | **POST - Tạo Đơn hàng**<br/>- Request Body: `{ items, shippingAddress, paymentMethod, couponCode }`<br/>- Status: 201 Created / 400 Bad Request |
| 51 | `/api/order/:id` | **GET - Chi tiết Đơn hàng**<br/>- Status: 200 OK / 404 Not Found |
| 52 | `/api/order/:id` | **PUT - Cập nhật Đơn hàng (Admin)**<br/>- Status: 200 OK / 403 Forbidden / 404 Not Found |
| 53 | `/api/order/:id` | **DELETE - Xóa Đơn hàng (Admin)**<br/>- Status: 200 OK / 403 Forbidden |
| 54 | `/api/order/:id/status` | **PUT - Cập nhật Trạng thái**<br/>- Request Body: `{ status: pending\|processing\|shipped\|delivered\|cancelled }`<br/>- Status: 200 OK |
| 55 | `/api/order/:id/confirm` | **PUT - Xác nhận Đơn hàng**<br/>- Status: 200 OK / 400 Bad Request |
| 56 | `/api/order/:id/cancel` | **PUT - Hủy Đơn hàng**<br/>- Status: 200 OK / 400 Bad Request |
| 57 | `/api/order/:id/history` | **GET - Lịch sử Đơn hàng**<br/>- Response: `{ history: Array }`<br/>- Status: 200 OK |

## 12. CUSTOM ORDER (7 APIs)

| STT | URL | Mô Tả Chi Tiết |
|-----|-----|---|
| 58 | `/api/custom/` | **GET - Danh sách Tùy chỉnh (Admin)**<br/>- Requires: `checkRole("admin")`<br/>- Status: 200 OK / 403 Forbidden |
| 59 | `/api/custom/` | **POST - Tạo Đơn Tùy chỉnh**<br/>- Request Body: `{ description, materials, gemstones, estimatedPrice }`<br/>- Status: 201 Created / 400 Bad Request |
| 60 | `/api/custom/:id` | **GET - Chi tiết Tùy chỉnh**<br/>- Status: 200 OK / 404 Not Found |
| 61 | `/api/custom/:id` | **PUT - Cập nhật Tùy chỉnh**<br/>- Status: 200 OK / 404 Not Found |
| 62 | `/api/custom/:id` | **DELETE - Xóa Tùy chỉnh**<br/>- Status: 200 OK / 404 Not Found |
| 63 | `/api/custom/:id/status` | **PUT - Cập nhật Trạng thái (Admin)**<br/>- Request Body: `{ status: pending\|processing\|completed\|rejected }`<br/>- Status: 200 OK / 403 Forbidden |
| 64 | `/api/custom/:id/confirm` | **PUT - Xác nhận Tùy chỉnh**<br/>- Status: 200 OK / 400 Bad Request |

## 13. REVIEW & RATING (5 APIs)

| STT | URL | Mô Tả Chi Tiết |
|-----|-----|---|
| 65 | `/api/review/` | **GET - Danh sách Review**<br/>- Query: `{ page, limit }`<br/>- Status: 200 OK |
| 66 | `/api/review/` | **POST - Tạo Review**<br/>- Request Body: `{ productSku, rating: 1-5, comment, images }`<br/>- Header: `Authorization: Bearer {token}`<br/>- Status: 201 Created / 400 Bad Request |
| 67 | `/api/review/product/:sku` | **GET - Review của Sản phẩm**<br/>- Response: `{ reviews: Array, averageRating }`<br/>- Status: 200 OK / 404 Not Found |
| 68 | `/api/review/:id` | **PUT - Cập nhật Review**<br/>- Status: 200 OK / 403 Forbidden / 404 Not Found |
| 69 | `/api/review/:id` | **DELETE - Xóa Review**<br/>- Status: 200 OK / 403 Forbidden |

## 14. WISHLIST (3 APIs)

| STT | URL | Mô Tả Chi Tiết |
|-----|-----|---|
| 70 | `/api/wish/` | **GET - Danh sách Yêu thích**<br/>- Header: `Authorization: Bearer {token}`<br/>- Response: `{ wishlist: Array, total }`<br/>- Status: 200 OK / 401 Unauthorized |
| 71 | `/api/wish/` | **POST - Thêm Yêu thích**<br/>- Request Body: `{ productSku }`<br/>- Status: 201 Created / 400 Bad Request |
| 72 | `/api/wish/:sku` | **DELETE - Xóa Yêu thích**<br/>- Status: 200 OK / 404 Not Found |

## 15. COUPON MANAGEMENT (3 APIs)

| STT | URL | Mô Tả Chi Tiết |
|-----|-----|---|
| 73 | `/api/coupon/` | **GET - Danh sách Coupon**<br/>- Status: 200 OK |
| 74 | `/api/coupon/` | **POST - Tạo Coupon (Admin)**<br/>- Request Body: `{ code, discount, expiryDate, maxUses }`<br/>- Status: 201 Created / 403 Forbidden |
| 75 | `/api/coupon/verify` | **POST - Xác thực Coupon**<br/>- Request Body: `{ code, totalAmount }`<br/>- Response: `{ valid: Boolean, discount: Number }`<br/>- Status: 200 OK / 400 Bad Request |

## 16. PAYMENT PROCESSING (4 APIs)

| STT | URL | Mô Tả Chi Tiết |
|-----|-----|---|
| 76 | `/api/payment/` | **POST - Tạo Payment Link (Thường)**<br/>- Request Body: `{ orderId, amount }`<br/>- Response: `{ paymentUrl }`<br/>- Status: 201 Created / 400 Bad Request |
| 77 | `/api/payment/custom` | **POST - Tạo Payment Link (Tùy chỉnh)**<br/>- Request Body: `{ customOrderId, amount }`<br/>- Status: 201 Created / 400 Bad Request |
| 78 | `/api/payment/success` | **POST - Callback Thành công (Thường)**<br/>- Webhook từ PayOS<br/>- Status: 200 OK |
| 79 | `/api/payment/success/custom` | **POST - Callback Thành công (Tùy chỉnh)**<br/>- Webhook từ PayOS<br/>- Status: 200 OK |

## 17. COMPARISON (4 APIs)

| STT | URL | Mô Tả Chi Tiết |
|-----|-----|---|
| 80 | `/api/compare/` | **GET - Danh sách So sánh**<br/>- Response: `{ compareList: Array }`<br/>- Status: 200 OK |
| 81 | `/api/compare/` | **POST - Thêm So sánh**<br/>- Request Body: `{ productSku }`<br/>- Status: 201 Created / 400 Bad Request |
| 82 | `/api/compare/:sku` | **DELETE - Xóa So sánh**<br/>- Status: 200 OK / 404 Not Found |
| 83 | `/api/compare/` | **DELETE - Xóa Tất cả So sánh**<br/>- Status: 200 OK |

## 18. CHATBOX & REAL-TIME (2 APIs)

| STT | URL | Mô Tả Chi Tiết |
|-----|-----|---|
| 84 | `/api/chatBox/` | **GET - Danh sách Tin nhắn**<br/>- Response: `{ messages: Array }`<br/>- Status: 200 OK |
| 85 | `/api/chatBox/` | **POST - Gửi Tin nhắn**<br/>- Request Body: `{ message, roomId }`<br/>- Status: 201 Created / 400 Bad Request |

## 19. PROVINCES & ADDRESS (2 APIs)

| STT | URL | Mô Tả Chi Tiết |
|-----|-----|---|
| 90 | `/api/provinces/` | **GET - Danh sách Tỉnh/Thành phố**<br/>- Response: `{ provinces: Array }`<br/>- Status: 200 OK |
| 91 | `/api/provinces/:province_code/communes` | **GET - Danh sách Xã/Phường**<br/>- URL Params: `province_code: String`<br/>- Response: `{ communes: Array }`<br/>- Status: 200 OK / 404 Not Found |

## 20. FILE MANAGEMENT & DASHBOARD (2 APIs)

| STT | URL | Mô Tả Chi Tiết |
|-----|-----|---|
| 92 | `/api/file/test-excel` | **POST - Upload & Parse Excel (Admin)**<br/>- Form: `file: File (.xlsx)`<br/>- Response: `{ sheetNames: Array, totalRows: Number, fullData: Array }`<br/>- Status: 200 OK / 400 Bad Request / 500 Internal Server Error |
| 93 | `/api/dashboard/` | **GET - Thống kê Dashboard (Admin)**<br/>- Requires: `checkRole("admin")`<br/>- Response: `{ totalOrders, totalRevenue, totalUsers, topProducts }`<br/>- Status: 200 OK / 403 Forbidden |

---

## Standard Response Format

### Success Response:
```json
{
  "success": true,
  "message": "Operation successful",
  "data": { /* API-specific data */ }
}
```

### Error Response:
```json
{
  "success": false,
  "message": "Error description",
  "error": { /* Error details */ }
}
```

## HTTP Status Codes

| Code | Meaning | Khi sử dụng |
|------|---------|-----------|
| 200 | OK | GET, PUT, DELETE thành công |
| 201 | Created | POST tạo resource thành công |
| 400 | Bad Request | Dữ liệu request không hợp lệ |
| 401 | Unauthorized | Không có JWT token hoặc token hết hạn |
| 403 | Forbidden | Không có quyền truy cập (admin required) |
| 404 | Not Found | Resource không tồn tại |
| 409 | Conflict | Dữ liệu trùng lặp (unique constraint) |
| 413 | Payload Too Large | File upload quá lớn |
| 500 | Internal Server Error | Lỗi server |

---

## Ghi Chú Bảo Mật

1. **JWT Authentication**: Tất cả API (ngoại trừ public endpoints) yêu cầu JWT token trong header `Authorization: Bearer {token}`
2. **Role-Based Access**: Admin-only endpoints kiểm tra role trong middleware `checkRole("admin")`
3. **Input Validation**: Tất cả request body được validate bằng Zod schemas
4. **SQL Injection Prevention**: Sử dụng MongoDB Mongoose với parameterized queries
5. **CORS**: Configured cho phép cross-origin requests từ client domain
6. **Rate Limiting**: Nên áp dụng rate limiting cho public endpoints
7. **Data Encryption**: Passwords được hash bằng bcrypt (min 10 rounds)
