# Cấu Trúc Thư Mục Client - Hệ Thống Jewelry

## Biểu Đồ Cấu Trúc Thư Mục

```
client/
├── node_modules/                  # Dependencies
├── public/                        # Static files
├── src/                          # Source code
│   ├── api/                      # API configuration
│   ├── assets/                   # Static assets (images, icons)
│   ├── components/               # Reusable React components
│   │   └── ui/                   # UI components
│   ├── hooks/                    # Custom React hooks
│   ├── layout/                   # Layout components
│   ├── lib/                      # Utility functions
│   ├── page/                     # Page components
│   │   ├── Admin/               # Admin pages
│   │   └── Account/             # Account/User pages
│   ├── route/                    # Router configuration
│   ├── service/                  # API services
│   ├── store/                    # State management (Zustand)
│   ├── App.jsx                   # Root component
│   ├── App.css                   # Global styles
│   ├── main.jsx                  # Entry point
│   ├── socket.js                 # Socket.io configuration
│   ├── index.css                 # Global CSS
│   └── google.js                 # Google OAuth config
├── .gitignore
├── components.json               # shadcn/ui config
├── eslint.config.js             # ESLint configuration
├── index.html                    # HTML template
├── jsconfig.json                # JavaScript config
├── package.json                 # Dependencies
├── package-lock.json            # Lock file
├── README.md                    # Documentation
└── vite.config.js              # Vite configuration
```

---

## 4.1.2 Danh Sách Các Thư Mục Chính - Số Thứ Tự, Tên Thư Mục, Mô Tả

| STT | Thành Phần | Mô Tả |
|-----|-----------|-------|
| 1 | src | Thư mục chứa toàn bộ mã nguồn chính của dự án |
| 2 | src/api | Thư mục chứa các file cấu hình API endpoints và constants |
| 3 | src/assets | Thư mục chứa các tài nguyên tĩnh không dùng trong dự án như: Ảnh, video, icon SVG |
| 4 | src/components | Thư mục chứa các file component React tái sử dụng trong dự án và có thể tái sử dụng |
| 5 | src/components/ui | Thư mục chứa giao diện cơ bản từ shadcn/ui như Button, Input, Card, Label, Radio Group, Separator, Field |
| 6 | src/hooks | Thư mục chứa các custom React hooks để xử lý logic logic chung |
| 7 | src/layout | Thư mục chứa các layout wrapper cho Account và Admin |
| 8 | src/lib | Thư mục chứa các hàm utility và helper functions |
| 9 | src/page | Thư mục chứa các page component - tương ứng với các routes |
| 10 | src/route | Thư mục chứa cấu hình Router và Protected Routes |
| 11 | src/service | Thư mục chứa API service layers để gọi API từ backend |
| 12 | src/store | Thư mục chứa các global state store sử dụng Zustand |

---

## 4.1.3 Chi Tiết Các Thư Mục Chính

### 1. **src/api** - API Configuration

| STT | Tên File | Chức Năng |
|-----|---------|----------|
| 1 | api.js | Chứa định nghĩa các API endpoint constants |

**Nội dung**: Danh sách URL endpoints (API_GOOGLE, API_AUTH, v.v.) được sử dụng trong toàn bộ ứng dụng.

---

### 2. **src/assets** - Static Assets

| STT | Tên File/Folder | Chức Năng |
|-----|----------------|----------|
| 1 | react.svg | Logo React (mẫu) |

**Nội dung**: Chứa các tài nguyên tĩnh như hình ảnh, icon, video không thay đổi.

---

### 3. **src/components** - Reusable Components

| STT | Tên Component | Chức Năng |
|-----|--------------|----------|
| 1 | Components/ui | Thư mục chứa UI components primitives |
| 2 | login-form.jsx | Form đăng nhập |
| 3 | signup-form.jsx | Form đăng ký |

**UI Components (src/components/ui/)**:
| STT | Tên File | Loại | Mô Tả |
|-----|---------|------|-------|
| 1 | button.jsx | Component | Nút bấm cơ bản |
| 2 | input.jsx | Component | Input field cơ bản |
| 3 | card.jsx | Component | Card container |
| 4 | label.jsx | Component | Label cho form fields |
| 5 | field.jsx | Component | Field wrapper |
| 6 | radio-group.jsx | Component | Radio button group |
| 7 | separator.jsx | Component | Separator line |

---

### 4. **src/hooks** - Custom React Hooks

| STT | Tên Hook | Chức Năng |
|-----|---------|----------|
| 1 | useGetListCategory.js | Lấy danh sách categories |
| 2 | useGetListSubcategory.js | Lấy danh sách subcategories |
| 3 | useGetListBrand.js | Lấy danh sách brands |
| 4 | useGetListProduct.js | Lấy danh sách sản phẩm |
| 5 | useGetListCart.js | Lấy dữ liệu giỏ hàng |
| 6 | useGetListOrder.js | Lấy danh sách đơn hàng |
| 7 | useGetListOrderByUserId.js | Lấy đơn hàng của user |
| 8 | useGetCustom.js | Lấy đơn hàng tùy chỉnh |
| 9 | useGetListCustomById.js | Lấy chi tiết custom order |
| 10 | useGetListReview.js | Lấy danh sách reviews |
| 11 | useGetListReviewByProductId.js | Lấy review theo sản phẩm |
| 12 | useGetListWish.js | Lấy danh sách wishlist |
| 13 | useGetListCompare.js | Lấy danh sách compare |
| 14 | useGetListCoupon.js | Lấy danh sách coupons |
| 15 | useGetListMaterial.js | Lấy danh sách materials |
| 16 | useGetListGemStone.js | Lấy danh sách gemstones |
| 17 | useGetListItem.js | Lấy danh sách items |
| 18 | useGetListProvinces.js | Lấy danh sách tỉnh/thành phố |
| 19 | useGetListCommunes.js | Lấy danh sách xã/phường |
| 20 | useGetListUser.js | Lấy danh sách users (admin) |
| 21 | useGetListDashboard.js | Lấy dữ liệu dashboard |
| 22 | useGetListChat.js | Lấy tin nhắn |
| 23 | useGetListChatBot.js | Lấy lịch sử chatbot |
| 24 | useGetListOnTime.js | Lấy sản phẩm mới |
| 25 | useCountDown.js | Hook countdown timer |

---

### 5. **src/layout** - Layout Components

| STT | Tên Layout | Chức Năng |
|-----|-----------|----------|
| 1 | LayoutAccount.jsx | Layout cho trang khách hàng |
| 2 | LayoutAdmin.jsx | Layout cho trang quản trị viên |

---

### 6. **src/lib** - Utility Functions

| STT | Tên File | Chức Năng |
|-----|---------|----------|
| 1 | utils.js | Utility functions (cn, format, etc.) |
| 2 | PaginationCustom.jsx | Pagination component |
| 3 | CurrencyInput.jsx | Currency input formatter |
| 4 | useInView.js | Hook detect element in view |
| 5 | format-big-number.js | Format large numbers |

---

### 7. **src/page** - Page Components

#### **Account Pages (src/page/Account/)**

| STT | Tên Component | Chức Năng |
|-----|--------------|----------|
| 1 | Home/Home.jsx | Trang chủ |
| 2 | Header/Header.jsx | Header navigation |
| 3 | Footer/Footer.jsx | Footer |
| 4 | Banner/Banner.jsx | Banner section |
| 5 | Featured/Featured.jsx | Featured products |
| 6 | Collections/Collections.jsx | Danh mục sản phẩm |
| 7 | Collections/FilterProduct.jsx | Lọc sản phẩm |
| 8 | SaleItem/SaleItem.jsx | Chi tiết sản phẩm |
| 9 | SaleItem/SaleItemList.jsx | Danh sách sản phẩm |
| 10 | SaleItem/SaleCard.jsx | Card sản phẩm |
| 11 | BestSeller/BestSeller.jsx | Sản phẩm bán chạy |
| 12 | DetailProduct/DetailProduct.jsx | Chi tiết sản phẩm |
| 13 | Cart/Cart.jsx | Giỏ hàng |
| 14 | Checkout/Checkout.jsx | Checkout page |
| 15 | Payment/PaymentList.jsx | Danh sách thanh toán |
| 16 | Payment/PaymentSuccess.jsx | Thanh toán thành công |
| 17 | Payment/PaymentSuccessCustom.jsx | Thanh toán custom thành công |
| 18 | Payment/PaymentFailed.jsx | Thanh toán thất bại |
| 19 | Profile/Profile.jsx | Trang hồ sơ |
| 20 | Profile/Personal.jsx | Thông tin cá nhân |
| 21 | Profile/OrderList.jsx | Danh sách đơn hàng |
| 22 | Profile/WishList.jsx | Danh sách yêu thích |
| 23 | Profile/DesignRequire.jsx | Yêu cầu thiết kế |
| 24 | Custom/Custom.jsx | Đặt hàng tùy chỉnh |
| 25 | Custom/JewelryType.jsx | Chọn loại trang sức |
| 26 | Custom/DesignPage.jsx | Trang thiết kế |
| 27 | Custom/InfomationPage.jsx | Thông tin đơn hàng |
| 28 | Custom/Preview3D.jsx | Preview 3D sản phẩm |
| 29 | Compare/Compare.jsx | So sánh sản phẩm |
| 30 | Wish/WishListPage.jsx | Trang danh sách yêu thích |
| 31 | ChatBox/ChatBox.jsx | Chat với admin |
| 32 | CustomerChat/CustomerChat.jsx | Chat customer |
| 33 | ServiceShip/service.jsx | Dịch vụ giao hàng |
| 34 | About/About.jsx | Về chúng tôi |
| 35 | OurStory/OurStory.jsx | Câu chuyện của chúng tôi |
| 36 | Stay/Stay.jsx | Subscribe newsletter |

#### **Admin Pages (src/page/Admin/)**

| STT | Tên Component | Chức Năng |
|-----|--------------|----------|
| 1 | Dashboard/Dashboard.jsx | Dashboard chính |
| 2 | Dashboard/SaleList.jsx | Danh sách bán hàng |
| 3 | category.jsx | Quản lý categories |
| 4 | subcategory.jsx | Quản lý subcategories |
| 5 | Brand.jsx | Quản lý brands |
| 6 | Items/Material.jsx | Quản lý materials |
| 7 | Items/GemStone.jsx | Quản lý gemstones |
| 8 | product/product.jsx | Danh sách sản phẩm |
| 9 | product/addProduct.jsx | Thêm sản phẩm |
| 10 | product/editProduct.jsx | Chỉnh sửa sản phẩm |
| 11 | product/PreviewModal.jsx | Preview modal sản phẩm |
| 12 | BoxProduct/BoxProduct.jsx | Quản lý sản phẩm chi tiết |
| 13 | BoxProduct/VariantOptions.jsx | Quản lý variants |
| 14 | Order/OrderListPage.jsx | Danh sách đơn hàng |
| 15 | Order/OrderChangeStatusModal.jsx | Thay đổi trạng thái đơn hàng |
| 16 | Coupon/Coupon.jsx | Quản lý coupons |
| 17 | Review/Review.jsx | Quản lý reviews |
| 18 | User/UserPage.jsx | Quản lý users |
| 19 | AdminChat/AdminChat.jsx | Chat với khách hàng |
| 20 | Requirement/Requirement.jsx | Yêu cầu từ khách hàng |

---

### 8. **src/route** - Router Configuration

| STT | Tên File | Chức Năng |
|-----|---------|----------|
| 1 | RouterAccount.jsx | Router cho account pages |
| 2 | RouterAdmin.jsx | Router cho admin pages |
| 3 | Protect/ProtectedRouter.jsx | Route guard cho user |
| 4 | Protect/ProtectAdmin.jsx | Route guard cho admin |

---

### 9. **src/service** - API Services

| STT | Tên Service | Chức Năng |
|-----|-----------|----------|
| 1 | axiosClient.js | Axios instance configuration |
| 2 | category/CategoryService.js | API calls cho category |
| 3 | subcategory/subcayegoryService.js | API calls cho subcategory |
| 4 | brand/BrandService.js | API calls cho brand |
| 5 | product/ProductService.js | API calls cho product |
| 6 | cart/CartService.js | API calls cho cart |
| 7 | order/orderService.js | API calls cho order |
| 8 | custom/CustomService.js | API calls cho custom order |
| 9 | review/reviewService.js | API calls cho review |
| 10 | wish/wishService.js | API calls cho wishlist |
| 11 | compare/compareService.js | API calls cho compare |
| 12 | coupon/CouponService.js | API calls cho coupon |
| 13 | material/MaterialService.js | API calls cho material |
| 14 | gemStone/GemStoneService.js | API calls cho gemstone |
| 15 | items/ItemService.js | API calls cho items |
| 16 | user/userService.js | API calls cho user management |
| 17 | dashboard/DashboardService.js | API calls cho dashboard |
| 18 | payment/payment.service.js | API calls cho payment |
| 19 | chatBox/ChatBoxService.js | API calls cho chatbox |
| 20 | Chat/ChatService.js | API calls cho chat |
| 21 | Provinces/provincesService.js | API calls cho provinces/communes |

---

### 10. **src/store** - State Management (Zustand)

| STT | Tên Store | Chức Năng |
|-----|-----------|----------|
| 1 | userAuthStore.js | Lưu trữ thông tin authentication |
| 2 | commonStore/commonStore.js | Lưu trữ common state |
| 3 | productStore/ProductStore.js | Lưu trữ product state |
| 4 | categoryStore/CategoryStore.js | Lưu trữ category state |
| 5 | subcategoryStore/subcategoryStore.js | Lưu trữ subcategory state |
| 6 | brandStore/BrandStore.js | Lưu trữ brand state |
| 7 | cartStore/CartStore.js | Lưu trữ cart state |
| 8 | orderStore/orderStore.js | Lưu trữ order state |
| 9 | customStore/CustomStore.js | Lưu trữ custom order state |
| 10 | reviewStore/reviewStore.js | Lưu trữ review state |
| 11 | wishStore/wishStore.js | Lưu trữ wishlist state |
| 12 | compareStore/CompareStore.js | Lưu trữ compare state |
| 13 | couponStore/CouponStore.js | Lưu trữ coupon state |
| 14 | materialStone/materialStone.js | Lưu trữ material state |
| 15 | gemStoneStore/gemStoneStore.js | Lưu trữ gemstone state |
| 16 | paymentStore/paymentStore.js | Lưu trữ payment state |
| 17 | chatBoxStore/ChatBoxStore.js | Lưu trữ chatbox state |
| 18 | ChatNotifyStore/ChatNotifyStore.js | Lưu trữ chat notifications |

---

### 11. **Root Level Files**

| STT | Tên File | Chức Năng |
|-----|---------|----------|
| 1 | App.jsx | Root component chứa Routes |
| 2 | main.jsx | Entry point React application |
| 3 | socket.js | Socket.io client configuration |
| 4 | google.js | Google OAuth configuration |
| 5 | App.css | Global app styles |
| 6 | index.css | Global CSS styles |

---

## Tóm Tắt Cấu Trúc

| Loại | Số Lượng |
|------|---------|
| Pages (Account) | 36 pages |
| Pages (Admin) | 20 pages |
| Components | 8 UI components + 2 form components |
| Custom Hooks | 25 hooks |
| Services | 21 API service files |
| Stores (Zustand) | 18 state stores |
| Routes | 4 route configuration files |
| Layouts | 2 layout components |
| **TOTAL** | **~160 files** |

---

## Flow Kiến Trúc Client

```
User (Browser)
    ↓
App.jsx (Root Component)
    ↓
Router (RouterAccount / RouterAdmin)
    ↓
Layout (LayoutAccount / LayoutAdmin)
    ↓
Pages & Components
    ↓
Custom Hooks (useGetList...)
    ↓
Services (APIService)
    ↓
Axios Client
    ↓
Backend API (Node.js Server)
    ↓
MongoDB
```

---

## Technologies Used

- **React 19**: UI framework
- **React Router 7**: Navigation
- **Vite**: Build tool
- **Tailwind CSS 4**: Styling
- **Material-UI 7**: Component library
- **Zustand 5**: State management
- **SWR 2**: Data fetching & caching
- **React Hook Form 7**: Form management
- **Zod 4**: Data validation
- **Socket.io Client 4**: Real-time communication
- **Axios 1**: HTTP client
- **React Slick**: Carousel
- **Three.js & React Three Fiber**: 3D visualization
- **Recharts**: Data visualization
- **Sonner & React Toastify**: Toast notifications
