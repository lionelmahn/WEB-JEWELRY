## 4.2.3 Thiết Kế Chi Tiết API

### Thiết Kế Chi Tiết Các API Endpoints

#### A. AUTHENTICATION SUBSYSTEM

| STT | URL | Mô Tả Chi Tiết |
|-----|-----|---|
| 1 | /api/auth/sign-up | **POST - Đăng ký tài khoản mới**<br/>- Mục đích: Tạo tài khoản người dùng mới<br/>- Request Body:<br/>&nbsp;&nbsp;+ email: String (required, unique)<br/>&nbsp;&nbsp;+ password: String (required, min 6 characters)<br/>&nbsp;&nbsp;+ fullName: String (optional)<br/>- Response:<br/>&nbsp;&nbsp;+ success: Boolean<br/>&nbsp;&nbsp;+ message: String<br/>&nbsp;&nbsp;+ data: { userId, email, token }<br/>- Status: 201 Created / 400 Bad Request / 409 Conflict |
| 2 | /api/auth/sign-in | **POST - Đăng nhập**<br/>- Mục đích: Xác thực và cấp phát JWT token<br/>- Request Body:<br/>&nbsp;&nbsp;+ email: String (required)<br/>&nbsp;&nbsp;+ password: String (required)<br/>- Response:<br/>&nbsp;&nbsp;+ success: Boolean<br/>&nbsp;&nbsp;+ message: String<br/>&nbsp;&nbsp;+ data: { userId, email, role, accessToken, refreshToken }<br/>- Status: 200 OK / 401 Unauthorized / 404 Not Found |
| 3 | /api/auth/sign-in-google | **POST - Đăng nhập Google**<br/>- Mục đích: Xác thực thông qua Google OAuth<br/>- Request Body:<br/>&nbsp;&nbsp;+ googleToken: String (required)<br/>- Response:<br/>&nbsp;&nbsp;+ success: Boolean<br/>&nbsp;&nbsp;+ data: { userId, email, accessToken, refreshToken }<br/>- Status: 200 OK / 400 Bad Request |
| 4 | /api/auth/refresh | **POST - Làm mới Token**<br/>- Mục đích: Cấp phát JWT token mới sử dụng refresh token<br/>- Request Header:<br/>&nbsp;&nbsp;+ Authorization: Bearer {refreshToken}<br/>- Response:<br/>&nbsp;&nbsp;+ success: Boolean<br/>&nbsp;&nbsp;+ data: { accessToken, refreshToken }<br/>- Status: 200 OK / 401 Unauthorized |

#### B. USER MANAGEMENT SUBSYSTEM

| STT | URL | Mô Tả Chi Tiết |
|-----|-----|---|
| 5 | /api/users | **GET - Danh sách người dùng**<br/>- Mục đích: Lấy danh sách tất cả người dùng (admin only)<br/>- Query Parameters:<br/>&nbsp;&nbsp;+ page: Number (optional, default 1)<br/>&nbsp;&nbsp;+ limit: Number (optional, default 10)<br/>&nbsp;&nbsp;+ search: String (optional)<br/>- Response:<br/>&nbsp;&nbsp;+ users: Array<br/>&nbsp;&nbsp;+ total: Number<br/>&nbsp;&nbsp;+ page: Number<br/>- Status: 200 OK / 401 Unauthorized / 403 Forbidden |
| 6 | /api/users/:id | **PUT - Cập nhật thông tin người dùng**<br/>- Mục đích: Cập nhật profile người dùng<br/>- URL Params:<br/>&nbsp;&nbsp;+ id: String (MongoDB ObjectId)<br/>- Request Body:<br/>&nbsp;&nbsp;+ fullName: String (optional)<br/>&nbsp;&nbsp;+ email: String (optional)<br/>&nbsp;&nbsp;+ phone: String (optional)<br/>&nbsp;&nbsp;+ address: String (optional)<br/>&nbsp;&nbsp;+ dateOfBirth: String (optional, ISO date)<br/>- Response:<br/>&nbsp;&nbsp;+ success: Boolean<br/>&nbsp;&nbsp;+ data: { updatedUser }<br/>- Status: 200 OK / 400 Bad Request / 404 Not Found |
| 7 | /api/users/update-role/:id | **PUT - Cập nhật vai trò người dùng**<br/>- Mục đích: Thay đổi role người dùng (admin only)<br/>- URL Params:<br/>&nbsp;&nbsp;+ id: String (MongoDB ObjectId)<br/>- Request Body:<br/>&nbsp;&nbsp;+ role: String (required, enum: user, admin)<br/>- Response:<br/>&nbsp;&nbsp;+ success: Boolean<br/>&nbsp;&nbsp;+ data: { updatedUser }<br/>- Status: 200 OK / 403 Forbidden / 404 Not Found |
| 8 | /api/users/logout | **GET - Đăng xuất**<br/>- Mục đích: Kết thúc phiên đăng nhập<br/>- Request Header:<br/>&nbsp;&nbsp;+ Authorization: Bearer {token}<br/>- Response:<br/>&nbsp;&nbsp;+ success: Boolean<br/>&nbsp;&nbsp;+ message: String<br/>- Status: 200 OK / 401 Unauthorized |
| 9 | /api/users/update-password | **POST - Cập nhật mật khẩu**<br/>- Mục đích: Thay đổi mật khẩu người dùng<br/>- Request Header:<br/>&nbsp;&nbsp;+ Authorization: Bearer {token}<br/>- Request Body:<br/>&nbsp;&nbsp;+ oldPassword: String (required)<br/>&nbsp;&nbsp;+ newPassword: String (required, min 6)<br/>&nbsp;&nbsp;+ confirmPassword: String (required)<br/>- Response:<br/>&nbsp;&nbsp;+ success: Boolean<br/>&nbsp;&nbsp;+ message: String<br/>- Status: 200 OK / 400 Bad Request / 401 Unauthorized |
| 10 | /api/users/upload | **POST - Upload Avatar**<br/>- Mục đích: Cập nhật ảnh đại diện người dùng<br/>- Request Header:<br/>&nbsp;&nbsp;+ Authorization: Bearer {token}<br/>&nbsp;&nbsp;+ Content-Type: multipart/form-data<br/>- Form Data:<br/>&nbsp;&nbsp;+ avatar: File (image/jpeg, image/png, max 5MB)<br/>- Response:<br/>&nbsp;&nbsp;+ success: Boolean<br/>&nbsp;&nbsp;+ data: { avatarUrl }<br/>- Status: 200 OK / 400 Bad Request / 413 Payload Too Large |
| 11 | /api/users/:id | **DELETE - Xoá người dùng**<br/>- Mục đích: Xoá tài khoản người dùng (admin only)<br/>- URL Params:<br/>&nbsp;&nbsp;+ id: String (MongoDB ObjectId)<br/>- Response:<br/>&nbsp;&nbsp;+ success: Boolean<br/>&nbsp;&nbsp;+ message: String<br/>- Status: 200 OK / 403 Forbidden / 404 Not Found |

#### C. PRODUCT MANAGEMENT SUBSYSTEM

| STT | URL | Mô Tả Chi Tiết |
|-----|-----|---|
| 12 | /api/products | **GET - Danh sách sản phẩm**<br/>- Mục đích: Lấy danh sách sản phẩm có sẵn (public)<br/>- Query Parameters:<br/>&nbsp;&nbsp;+ page: Number (optional, default 1)<br/>&nbsp;&nbsp;+ limit: Number (optional, default 10)<br/>&nbsp;&nbsp;+ category: String (optional)<br/>&nbsp;&nbsp;+ brand: String (optional)<br/>&nbsp;&nbsp;+ search: String (optional)<br/>&nbsp;&nbsp;+ sortBy: String (optional, price, rating, newest)<br/>- Response:<br/>&nbsp;&nbsp;+ products: Array<br/>&nbsp;&nbsp;+ total: Number<br/>- Status: 200 OK |
| 13 | /api/products/date-time | **GET - Sản phẩm theo thời gian**<br/>- Mục đích: Lấy sản phẩm mới nhất hoặc trending<br/>- Query Parameters:<br/>&nbsp;&nbsp;+ range: String (optional, week, month, year)<br/>- Response:<br/>&nbsp;&nbsp;+ products: Array<br/>- Status: 200 OK |
| 14 | /api/products/:id | **GET - Chi tiết sản phẩm**<br/>- Mục đích: Lấy thông tin chi tiết một sản phẩm<br/>- URL Params:<br/>&nbsp;&nbsp;+ id: String (MongoDB ObjectId)<br/>- Response:<br/>&nbsp;&nbsp;+ product: Object { id, name, price, description, images, category, brand, rating, reviews }<br/>- Status: 200 OK / 404 Not Found |
| 15 | /api/products/edit/:id | **GET - Sản phẩm để chỉnh sửa**<br/>- Mục đích: Lấy thông tin sản phẩm cho form edit (admin only)<br/>- URL Params:<br/>&nbsp;&nbsp;+ id: String (MongoDB ObjectId)<br/>- Response:<br/>&nbsp;&nbsp;+ product: Object (toàn bộ thông tin)<br/>- Status: 200 OK / 403 Forbidden / 404 Not Found |
| 16 | /api/products | **POST - Tạo sản phẩm**<br/>- Mục đích: Tạo sản phẩm mới (admin only)<br/>- Request Body:<br/>&nbsp;&nbsp;+ name: String (required)<br/>&nbsp;&nbsp;+ description: String (optional)<br/>&nbsp;&nbsp;+ price: Number (required)<br/>&nbsp;&nbsp;+ category: String (required, ObjectId)<br/>&nbsp;&nbsp;+ brand: String (optional, ObjectId)<br/>&nbsp;&nbsp;+ sku: String (required, unique)<br/>&nbsp;&nbsp;+ stock: Number (required)<br/>&nbsp;&nbsp;+ images: Array (optional)<br/>- Response:<br/>&nbsp;&nbsp;+ success: Boolean<br/>&nbsp;&nbsp;+ data: { productId, name, price }<br/>- Status: 201 Created / 400 Bad Request / 403 Forbidden |
| 17 | /api/products/:id | **PUT - Cập nhật sản phẩm**<br/>- Mục đích: Cập nhật thông tin sản phẩm (admin only)<br/>- URL Params:<br/>&nbsp;&nbsp;+ id: String (MongoDB ObjectId)<br/>- Request Body: (tương tự create, tất cả optional)<br/>- Response:<br/>&nbsp;&nbsp;+ success: Boolean<br/>&nbsp;&nbsp;+ data: { updatedProduct }<br/>- Status: 200 OK / 400 Bad Request / 404 Not Found |
| 18 | /api/products/upload | **POST - Upload hình ảnh**<br/>- Mục đích: Upload hình ảnh sản phẩm (admin only)<br/>- Request Header:<br/>&nbsp;&nbsp;+ Content-Type: multipart/form-data<br/>- Form Data:<br/>&nbsp;&nbsp;+ product-images: File[] (max 10 files, max 5MB each)<br/>- Response:<br/>&nbsp;&nbsp;+ success: Boolean<br/>&nbsp;&nbsp;+ data: { imageUrls: Array }<br/>- Status: 200 OK / 400 Bad Request |
| 19 | /api/products/file-excel | **POST - Import Excel**<br/>- Mục đích: Tải lên file Excel để import sản phẩm (admin only)<br/>- Request Header:<br/>&nbsp;&nbsp;+ Content-Type: multipart/form-data<br/>- Form Data:<br/>&nbsp;&nbsp;+ file-excel: File (.xlsx, max 10MB)<br/>- Response:<br/>&nbsp;&nbsp;+ success: Boolean<br/>&nbsp;&nbsp;+ message: String<br/>- Status: 200 OK / 400 Bad Request |
| 20 | /api/products/file-excel-preview | **POST - Xem trước Excel**<br/>- Mục đích: Xem trước dữ liệu trước khi import (admin only)<br/>- Request Header:<br/>&nbsp;&nbsp;+ Content-Type: multipart/form-data<br/>- Form Data:<br/>&nbsp;&nbsp;+ file-excel: File (.xlsx)<br/>- Response:<br/>&nbsp;&nbsp;+ data: { preview: Array, totalRows: Number }<br/>- Status: 200 OK / 400 Bad Request |
| 21 | /api/products/delete-upload | **DELETE - Xoá hình tạm thời**<br/>- Mục đích: Xoá hình ảnh tạm thời từ uploads (admin only)<br/>- Request Body:<br/>&nbsp;&nbsp;+ imageUrl: String (required)<br/>- Response:<br/>&nbsp;&nbsp;+ success: Boolean<br/>- Status: 200 OK / 400 Bad Request |
| 22 | /api/products/:id | **DELETE - Xoá sản phẩm**<br/>- Mục đích: Xoá sản phẩm khỏi hệ thống (admin only)<br/>- URL Params:<br/>&nbsp;&nbsp;+ id: String (MongoDB ObjectId)<br/>- Response:<br/>&nbsp;&nbsp;+ success: Boolean<br/>- Status: 200 OK / 403 Forbidden / 404 Not Found |
| 23 | /api/products/:id/image | **DELETE - Xoá hình ảnh**<br/>- Mục đích: Xoá một hình ảnh từ sản phẩm (admin only)<br/>- URL Params:<br/>&nbsp;&nbsp;+ id: String (MongoDB ObjectId)<br/>- Request Body:<br/>&nbsp;&nbsp;+ imageUrl: String (required)<br/>- Response:<br/>&nbsp;&nbsp;+ success: Boolean<br/>- Status: 200 OK / 400 Bad Request |

#### D. SHOPPING CART SUBSYSTEM

| STT | URL | Mô Tả Chi Tiết |
|-----|-----|---|
| 24 | /api/cart | **GET - Lấy giỏ hàng**<br/>- Mục đích: Lấy danh sách sản phẩm trong giỏ hàng (authenticated)<br/>- Request Header:<br/>&nbsp;&nbsp;+ Authorization: Bearer {token}<br/>- Response:<br/>&nbsp;&nbsp;+ cart: Array<br/>&nbsp;&nbsp;+ totalPrice: Number<br/>&nbsp;&nbsp;+ totalItems: Number<br/>- Status: 200 OK / 401 Unauthorized |
| 25 | /api/cart | **POST - Thêm vào giỏ hàng**<br/>- Mục đích: Thêm sản phẩm vào giỏ hàng<br/>- Request Header:<br/>&nbsp;&nbsp;+ Authorization: Bearer {token}<br/>- Request Body:<br/>&nbsp;&nbsp;+ productId: String (required, ObjectId)<br/>&nbsp;&nbsp;+ quantity: Number (required, min 1)<br/>&nbsp;&nbsp;+ sku: String (required)<br/>- Response:<br/>&nbsp;&nbsp;+ success: Boolean<br/>&nbsp;&nbsp;+ data: { cartItem }<br/>- Status: 201 Created / 400 Bad Request |
| 26 | /api/cart/:sku | **PATCH - Cập nhật số lượng**<br/>- Mục đích: Cập nhật số lượng sản phẩm trong giỏ<br/>- URL Params:<br/>&nbsp;&nbsp;+ sku: String (product SKU)<br/>- Request Header:<br/>&nbsp;&nbsp;+ Authorization: Bearer {token}<br/>- Request Body:<br/>&nbsp;&nbsp;+ quantity: Number (required)<br/>- Response:<br/>&nbsp;&nbsp;+ success: Boolean<br/>&nbsp;&nbsp;+ data: { updatedItem }<br/>- Status: 200 OK / 400 Bad Request / 404 Not Found |
| 27 | /api/cart/:sku | **DELETE - Xoá sản phẩm khỏi giỏ**<br/>- Mục đích: Xoá một sản phẩm từ giỏ hàng<br/>- URL Params:<br/>&nbsp;&nbsp;+ sku: String (product SKU)<br/>- Response:<br/>&nbsp;&nbsp;+ success: Boolean<br/>- Status: 200 OK / 404 Not Found |
| 28 | /api/cart/clear/:sku | **DELETE - Xoá toàn bộ giỏ**<br/>- Mục đích: Xoá tất cả sản phẩm trong giỏ<br/>- URL Params:<br/>&nbsp;&nbsp;+ sku: String (hoặc 'all')<br/>- Response:<br/>&nbsp;&nbsp;+ success: Boolean<br/>- Status: 200 OK |

#### E. ORDER PROCESSING SUBSYSTEM

| STT | URL | Mô Tả Chi Tiết |
|-----|-----|---|
| 29 | /api/orders/list-order | **GET - Đơn hàng của tôi**<br/>- Mục đích: Lấy danh sách đơn hàng của người dùng (authenticated)<br/>- Request Header:<br/>&nbsp;&nbsp;+ Authorization: Bearer {token}<br/>- Query Parameters:<br/>&nbsp;&nbsp;+ status: String (optional, pending, processing, shipped, delivered)<br/>&nbsp;&nbsp;+ page: Number (optional)<br/>- Response:<br/>&nbsp;&nbsp;+ orders: Array<br/>&nbsp;&nbsp;+ total: Number<br/>- Status: 200 OK / 401 Unauthorized |
| 30 | /api/orders | **GET - Tất cả đơn hàng**<br/>- Mục đích: Lấy danh sách tất cả đơn hàng (admin only)<br/>- Query Parameters:<br/>&nbsp;&nbsp;+ status: String (optional)<br/>&nbsp;&nbsp;+ paymentStatus: String (optional)<br/>&nbsp;&nbsp;+ page: Number (optional)<br/>- Response:<br/>&nbsp;&nbsp;+ orders: Array<br/>&nbsp;&nbsp;+ total: Number<br/>- Status: 200 OK / 403 Forbidden |
| 31 | /api/orders/preview | **POST - Xem trước đơn hàng**<br/>- Mục đích: Tính toán tổng tiền trước khi đặt hàng<br/>- Request Body:<br/>&nbsp;&nbsp;+ items: Array (required)<br/>&nbsp;&nbsp;+ couponCode: String (optional)<br/>- Response:<br/>&nbsp;&nbsp;+ subtotal: Number<br/>&nbsp;&nbsp;+ discount: Number<br/>&nbsp;&nbsp;+ tax: Number<br/>&nbsp;&nbsp;+ total: Number<br/>- Status: 200 OK / 400 Bad Request |
| 32 | /api/orders/use-coupon | **POST - Áp dụng mã giảm giá**<br/>- Mục đích: Kiểm tra và áp dụng coupon<br/>- Request Body:<br/>&nbsp;&nbsp;+ couponCode: String (required)<br/>&nbsp;&nbsp;+ totalAmount: Number (required)<br/>- Response:<br/>&nbsp;&nbsp;+ valid: Boolean<br/>&nbsp;&nbsp;+ discount: Number<br/>&nbsp;&nbsp;+ message: String<br/>- Status: 200 OK / 400 Bad Request |
| 33 | /api/orders | **POST - Tạo đơn hàng**<br/>- Mục đích: Tạo đơn hàng mới (authenticated)<br/>- Request Header:<br/>&nbsp;&nbsp;+ Authorization: Bearer {token}<br/>- Request Body:<br/>&nbsp;&nbsp;+ items: Array (required)<br/>&nbsp;&nbsp;+ shippingAddress: Object (required)<br/>&nbsp;&nbsp;+ paymentMethod: String (required, card, transfer)<br/>&nbsp;&nbsp;+ couponCode: String (optional)<br/>- Response:<br/>&nbsp;&nbsp;+ success: Boolean<br/>&nbsp;&nbsp;+ data: { orderId, totalAmount, paymentUrl }<br/>- Status: 201 Created / 400 Bad Request |
| 34 | /api/orders/:id | **GET - Chi tiết đơn hàng**<br/>- Mục đích: Lấy thông tin chi tiết đơn hàng<br/>- URL Params:<br/>&nbsp;&nbsp;+ id: String (MongoDB ObjectId)<br/>- Response:<br/>&nbsp;&nbsp;+ order: Object<br/>- Status: 200 OK / 404 Not Found |
| 35 | /api/orders/:id/status | **PUT - Cập nhật trạng thái**<br/>- Mục đích: Cập nhật trạng thái đơn hàng (admin only)<br/>- URL Params:<br/>&nbsp;&nbsp;+ id: String<br/>- Request Body:<br/>&nbsp;&nbsp;+ status: String (required, pending, processing, shipped, delivered, cancelled)<br/>- Response:<br/>&nbsp;&nbsp;+ success: Boolean<br/>- Status: 200 OK / 403 Forbidden |
| 36 | /api/orders/:id/payment-status | **PUT - Cập nhật trạng thái thanh toán**<br/>- Mục đích: Cập nhật trạng thái thanh toán<br/>- URL Params:<br/>&nbsp;&nbsp;+ id: String<br/>- Request Body:<br/>&nbsp;&nbsp;+ paymentStatus: String (required, pending, completed, failed)<br/>- Response:<br/>&nbsp;&nbsp;+ success: Boolean<br/>- Status: 200 OK / 400 Bad Request |
| 37 | /api/orders/:id | **DELETE - Huỷ đơn hàng**<br/>- Mục đích: Huỷ đơn hàng<br/>- URL Params:<br/>&nbsp;&nbsp;+ id: String<br/>- Response:<br/>&nbsp;&nbsp;+ success: Boolean<br/>&nbsp;&nbsp;+ message: String<br/>- Status: 200 OK / 400 Bad Request / 404 Not Found |

#### F. CUSTOM ORDER SUBSYSTEM

| STT | URL | Mô Tả Chi Tiết |
|-----|-----|---|
| 38 | /api/custom | **GET - Danh sách tùy chỉnh (Admin)**<br/>- Mục đích: Lấy danh sách đơn hàng tùy chỉnh (admin only)<br/>- Query Parameters:<br/>&nbsp;&nbsp;+ status: String (optional)<br/>&nbsp;&nbsp;+ page: Number (optional)<br/>- Response:<br/>&nbsp;&nbsp;+ customs: Array<br/>&nbsp;&nbsp;+ total: Number<br/>- Status: 200 OK / 403 Forbidden |
| 39 | /api/custom/user | **GET - Đơn hàng tùy chỉnh của tôi**<br/>- Mục đích: Lấy danh sách đơn tùy chỉnh của người dùng<br/>- Request Header:<br/>&nbsp;&nbsp;+ Authorization: Bearer {token}<br/>- Response:<br/>&nbsp;&nbsp;+ customs: Array<br/>- Status: 200 OK / 401 Unauthorized |
| 40 | /api/custom | **POST - Tạo đơn tùy chỉnh**<br/>- Mục đích: Tạo đơn hàng tùy chỉnh<br/>- Request Header:<br/>&nbsp;&nbsp;+ Authorization: Bearer {token}<br/>- Request Body:<br/>&nbsp;&nbsp;+ description: String (required)<br/>&nbsp;&nbsp;+ materials: Array (required)<br/>&nbsp;&nbsp;+ gemstones: Array (optional)<br/>&nbsp;&nbsp;+ estimatedPrice: Number (optional)<br/>- Response:<br/>&nbsp;&nbsp;+ success: Boolean<br/>&nbsp;&nbsp;+ data: { customOrderId }<br/>- Status: 201 Created / 400 Bad Request |
| 41 | /api/custom/calculate | **POST - Tính giá tùy chỉnh**<br/>- Mục đích: Tính giá đơn hàng tùy chỉnh<br/>- Request Body:<br/>&nbsp;&nbsp;+ materials: Array (required)<br/>&nbsp;&nbsp;+ gemstones: Array (optional)<br/>&nbsp;&nbsp;+ weight: Number (optional)<br/>- Response:<br/>&nbsp;&nbsp;+ price: Number<br/>&nbsp;&nbsp;+ breakdown: Object<br/>- Status: 200 OK / 400 Bad Request |
| 42 | /api/custom/preview/:id | **POST - Xem trước thiết kế**<br/>- Mục đích: Tạo preview 3D thiết kế tùy chỉnh<br/>- URL Params:<br/>&nbsp;&nbsp;+ id: String (customOrder ID)<br/>- Response:<br/>&nbsp;&nbsp;+ previewUrl: String<br/>&nbsp;&nbsp;+ success: Boolean<br/>- Status: 200 OK / 404 Not Found |
| 43 | /api/custom/update/:id | **PUT - Cập nhật đơn tùy chỉnh**<br/>- Mục đích: Cập nhật thông tin đơn tùy chỉnh<br/>- URL Params:<br/>&nbsp;&nbsp;+ id: String<br/>- Request Body: (tương tự create, tất cả optional)<br/>- Response:<br/>&nbsp;&nbsp;+ success: Boolean<br/>&nbsp;&nbsp;+ data: { updatedCustom }<br/>- Status: 200 OK / 404 Not Found |
| 44 | /api/custom/:id | **PUT - Cập nhật trạng thái tùy chỉnh**<br/>- Mục đích: Cập nhật trạng thái xử lý (admin only)<br/>- URL Params:<br/>&nbsp;&nbsp;+ id: String<br/>- Request Body:<br/>&nbsp;&nbsp;+ status: String (required, pending, processing, completed, rejected)<br/>- Response:<br/>&nbsp;&nbsp;+ success: Boolean<br/>- Status: 200 OK / 403 Forbidden |

#### G. REVIEW & RATING SUBSYSTEM

| STT | URL | Mô Tả Chi Tiết |
|-----|-----|---|
| 45 | /api/reviews/all | **GET - Tất cả đánh giá**<br/>- Mục đích: Lấy danh sách tất cả đánh giá (admin)<br/>- Query Parameters:<br/>&nbsp;&nbsp;+ page: Number (optional)<br/>&nbsp;&nbsp;+ rating: Number (optional, 1-5)<br/>- Response:<br/>&nbsp;&nbsp;+ reviews: Array<br/>&nbsp;&nbsp;+ total: Number<br/>- Status: 200 OK / 403 Forbidden |
| 46 | /api/reviews | **GET - Đánh giá theo sản phẩm**<br/>- Mục đích: Lấy đánh giá của một sản phẩm<br/>- Query Parameters:<br/>&nbsp;&nbsp;+ productId: String (required)<br/>&nbsp;&nbsp;+ page: Number (optional)<br/>- Response:<br/>&nbsp;&nbsp;+ reviews: Array<br/>&nbsp;&nbsp;+ averageRating: Number<br/>- Status: 200 OK / 400 Bad Request |
| 47 | /api/reviews | **POST - Tạo đánh giá**<br/>- Mục đích: Tạo đánh giá sản phẩm (authenticated)<br/>- Request Header:<br/>&nbsp;&nbsp;+ Authorization: Bearer {token}<br/>- Request Body:<br/>&nbsp;&nbsp;+ productId: String (required, ObjectId)<br/>&nbsp;&nbsp;+ rating: Number (required, 1-5)<br/>&nbsp;&nbsp;+ comment: String (optional)<br/>&nbsp;&nbsp;+ images: Array (optional)<br/>- Response:<br/>&nbsp;&nbsp;+ success: Boolean<br/>&nbsp;&nbsp;+ data: { reviewId }<br/>- Status: 201 Created / 400 Bad Request |
| 48 | /api/reviews/:id | **PUT - Cập nhật đánh giá**<br/>- Mục đích: Cập nhật đánh giá của mình<br/>- URL Params:<br/>&nbsp;&nbsp;+ id: String (Review ID)<br/>- Request Body:<br/>&nbsp;&nbsp;+ rating: Number (optional)<br/>&nbsp;&nbsp;+ comment: String (optional)<br/>- Response:<br/>&nbsp;&nbsp;+ success: Boolean<br/>- Status: 200 OK / 403 Forbidden / 404 Not Found |
| 49 | /api/reviews/:id | **DELETE - Xoá đánh giá**<br/>- Mục đích: Xoá đánh giá của mình<br/>- URL Params:<br/>&nbsp;&nbsp;+ id: String (Review ID)<br/>- Response:<br/>&nbsp;&nbsp;+ success: Boolean<br/>- Status: 200 OK / 403 Forbidden / 404 Not Found |

#### H. WISHLIST SUBSYSTEM

| STT | URL | Mô Tả Chi Tiết |
|-----|-----|---|
| 50 | /api/wishlist | **GET - Danh sách yêu thích**<br/>- Mục đích: Lấy danh sách sản phẩm yêu thích (authenticated)<br/>- Request Header:<br/>&nbsp;&nbsp;+ Authorization: Bearer {token}<br/>- Response:<br/>&nbsp;&nbsp;+ wishlist: Array<br/>&nbsp;&nbsp;+ total: Number<br/>- Status: 200 OK / 401 Unauthorized |
| 51 | /api/wishlist | **POST - Thêm vào yêu thích**<br/>- Mục đích: Thêm sản phẩm vào danh sách yêu thích<br/>- Request Header:<br/>&nbsp;&nbsp;+ Authorization: Bearer {token}<br/>- Request Body:<br/>&nbsp;&nbsp;+ productId: String (required, ObjectId)<br/>- Response:<br/>&nbsp;&nbsp;+ success: Boolean<br/>&nbsp;&nbsp;+ message: String<br/>- Status: 201 Created / 400 Bad Request |
| 52 | /api/wishlist/:id | **DELETE - Xoá khỏi yêu thích**<br/>- Mục đích: Xoá sản phẩm khỏi danh sách yêu thích<br/>- URL Params:<br/>&nbsp;&nbsp;+ id: String (Wishlist/Product ID)<br/>- Response:<br/>&nbsp;&nbsp;+ success: Boolean<br/>- Status: 200 OK / 404 Not Found |

#### I. COUPON MANAGEMENT SUBSYSTEM

| STT | URL | Mô Tả Chi Tiết |
|-----|-----|---|
| 53 | /api/coupons | **GET - Danh sách mã giảm giá**<br/>- Mục đích: Lấy danh sách coupon có sẵn<br/>- Query Parameters:<br/>&nbsp;&nbsp;+ valid: Boolean (optional, true/false)<br/>&nbsp;&nbsp;+ page: Number (optional)<br/>- Response:<br/>&nbsp;&nbsp;+ coupons: Array<br/>&nbsp;&nbsp;+ total: Number<br/>- Status: 200 OK |
| 54 | /api/coupons | **POST - Tạo mã giảm giá**<br/>- Mục đích: Tạo coupon mới (admin only)<br/>- Request Body:<br/>&nbsp;&nbsp;+ code: String (required, unique)<br/>&nbsp;&nbsp;+ discountType: String (required, percentage, fixed)<br/>&nbsp;&nbsp;+ discountValue: Number (required)<br/>&nbsp;&nbsp;+ expiryDate: Date (optional)<br/>&nbsp;&nbsp;+ maxUses: Number (optional)<br/>- Response:<br/>&nbsp;&nbsp;+ success: Boolean<br/>&nbsp;&nbsp;+ data: { couponId }<br/>- Status: 201 Created / 400 Bad Request |
| 55 | /api/coupons/:id | **PUT - Cập nhật mã giảm giá**<br/>- Mục đích: Cập nhật coupon (admin only)<br/>- URL Params:<br/>&nbsp;&nbsp;+ id: String<br/>- Request Body: (tương tự create, tất cả optional)<br/>- Response:<br/>&nbsp;&nbsp;+ success: Boolean<br/>- Status: 200 OK / 403 Forbidden |
| 56 | /api/coupons/:id | **DELETE - Xoá mã giảm giá**<br/>- Mục đích: Xoá coupon (admin only)<br/>- URL Params:<br/>&nbsp;&nbsp;+ id: String<br/>- Response:<br/>&nbsp;&nbsp;+ success: Boolean<br/>- Status: 200 OK / 403 Forbidden |

#### J. REAL-TIME COMMUNICATION SUBSYSTEM

| STT | URL | Mô Tả Chi Tiết |
|-----|-----|---|
| 57 | /api/conversation | **GET - Tất cả tin nhắn**<br/>- Mục đích: Lấy danh sách tất cả cuộc trò chuyện<br/>- Query Parameters:<br/>&nbsp;&nbsp;+ page: Number (optional)<br/>- Response:<br/>&nbsp;&nbsp;+ conversations: Array<br/>- Status: 200 OK |
| 58 | /api/conversation/me | **GET - Tin nhắn của tôi**<br/>- Mục đích: Lấy cuộc trò chuyện của người dùng (authenticated)<br/>- Request Header:<br/>&nbsp;&nbsp;+ Authorization: Bearer {token}<br/>- Response:<br/>&nbsp;&nbsp;+ messages: Array<br/>- Status: 200 OK / 401 Unauthorized |
| 59 | /api/conversation/hasunread | **GET - Kiểm tra tin chưa đọc**<br/>- Mục đích: Kiểm tra có tin nhắn chưa đọc (authenticated)<br/>- Response:<br/>&nbsp;&nbsp;+ hasUnread: Boolean<br/>&nbsp;&nbsp;+ unreadCount: Number<br/>- Status: 200 OK |
| 60 | /api/conversation/markread | **GET - Đánh dấu đã đọc**<br/>- Mục đích: Đánh dấu tin nhắn đã đọc (authenticated)<br/>- Response:<br/>&nbsp;&nbsp;+ success: Boolean<br/>- Status: 200 OK |

### Quy Ước Response Format

Tất cả API responses đều tuân theo định dạng chuẩn:

```json
{
  "success": boolean,
  "message": "string",
  "data": {
    // response data
  },
  "error": null // hoặc error object
}
```

### HTTP Status Codes

- **2xx Success**
  - 200 OK: Thành công
  - 201 Created: Tạo mới thành công
  
- **4xx Client Error**
  - 400 Bad Request: Dữ liệu không hợp lệ
  - 401 Unauthorized: Không được xác thực
  - 403 Forbidden: Không có quyền truy cập
  - 404 Not Found: Không tìm thấy tài nguyên
  - 409 Conflict: Xung đột dữ liệu
  - 413 Payload Too Large: File quá lớn
  
- **5xx Server Error**
  - 500 Internal Server Error: Lỗi máy chủ
