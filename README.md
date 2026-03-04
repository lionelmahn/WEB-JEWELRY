# Jewelry Management System

Hệ thống quản lý bán hàng trang sức toàn diện được xây dựng với **React**, **Node.js** và **MongoDB**. Ứng dụng cung cấp các tính năng như quản lý sản phẩm, giỏ hàng, đơn hàng, thanh toán, và chat real-time.

## Tính Năng Chính

- **Quản lý Sản Phẩm**: CRUD sản phẩm, phân loại, thương hiệu, chất liệu, đá quý
- **Hệ Thống Đơn Hàng**: Đơn hàng thường và tùy chỉnh
- **Giỏ Hàng & Thanh Toán**: Tích hợp PayOS, xác thực coupon
- **Danh Sách Yêu Thích & So Sánh**: Lưu sản phẩm yêu thích và so sánh
- **Review & Rating**: Đánh giá sản phẩm từ khách hàng
- **Chat Real-time**: Socket.io cho tính năng nhắn tin trực tiếp
- **Dashboard Admin**: Thống kê và quản lý hệ thống
- **Tùy Chỉnh Sản Phẩm**: Cho phép khách hàng đặt hàng tùy chỉnh
- **Upload File**: Hỗ trợ upload Excel để nhập sản phẩm hàng loạt

## Yêu Cầu Hệ Thống

- **Node.js**: v16.x hoặc cao hơn
- **npm** hoặc **yarn**: Để quản lý packages
- **MongoDB**: Database (local hoặc cloud - MongoDB Atlas)
- **Git**: Để clone repository

## Hướng Dẫn Cài Đặt

### 1. Clone Repository

```bash
git clone https://github.com/lionelmahn/WEB-JEWELRY.git
cd WEB-JEWELRY
```

### 2. Cài Đặt Dependencies

#### Cài đặt Server Dependencies

```bash
cd server
npm install
```

#### Cài đặt Client Dependencies

```bash
cd ../client
npm install
```

### 3. Cấu Hình Environment Variables

#### Server Configuration

Tạo file `.env` trong thư mục `server/` dựa trên file `.env.example`:

```bash
cd server
cp .env.example .env
```

Sau đó, mở file `.env` và cấu hình các biến sau:

```env
# MongoDB Connection String
MONGO_URL=mongodb://localhost:27017/jewelry
# Hoặc sử dụng MongoDB Atlas:
# MONGO_URL=mongodb+srv://username:password@cluster0.mongodb.net/jewelry?retryWrites=true&w=majority

# Cloudinary (Lưu trữ ảnh)
CLOUDINARY_NAME=your_cloudinary_name
CLOUDINARY_KEY=your_cloudinary_key
CLOUDINARY_SECRET=your_cloudinary_secret

# JWT Secret (Tạo chuỗi random cho JWT)
JWT_SECRET=your_secret_key_here

# OpenAI API Key (Cho chatbot - Optional)
OPENAI_API_KEY=your_openai_api_key

# PayOS API (Cho thanh toán)
PAYOS_CLIENT_ID=your_payos_client_id
PAYOS_API_KEY=your_payos_api_key
PAYOS_CHECKSUM_KEY=your_payos_checksum_key
```

**Hướng dẫn lấy các key:**

- **MongoDB**: 
  - Local: Cài MongoDB Community Edition
  - Cloud: Tạo account tại https://www.mongodb.com/cloud/atlas

- **Cloudinary**: 
  - Đăng ký tại https://cloudinary.com
  - Lấy credentials từ Dashboard

- **PayOS**: 
  - Đăng ký tại https://payos.vn
  - Lấy API keys từ Developer Settings

- **OpenAI** (Optional):
  - Đăng ký tại https://platform.openai.com
  - Lấy API key từ Account Settings

#### Client Configuration

Client không cần file `.env`, nhưng bạn có thể tạo file `.env.local` nếu cần custom API URL:

```env
VITE_API_URL=http://localhost:3000/api
```

### 4. Chạy Ứng Dụng

#### Chạy Server (Terminal 1)

```bash
cd server
npm run dev
```

Server sẽ chạy tại: `http://localhost:3000`

#### Chạy Client (Terminal 2)

```bash
cd client
npm run dev
```

Client sẽ chạy tại: `http://localhost:5173`

### 5. Mở Ứng Dụng

Truy cập vào http://localhost:5173 trên trình duyệt

## Cấu Trúc Dự Án

```
WEB-JEWELRY/
├── server/                      # Backend (Node.js + Express)
│   ├── src/
│   │   ├── server.js           # File khởi tạo server
│   │   ├── config/             # Cấu hình (Database, v.v.)
│   │   ├── controller/         # Controller (xử lý business logic)
│   │   ├── routes/             # API routes (22 files)
│   │   ├── models/             # MongoDB models (15 models)
│   │   ├── middleware/         # Middleware (Auth, validation)
│   │   ├── auth/               # Authentication logic
│   │   ├── libs/               # Utility functions
│   │   └── utils/              # Helper functions
│   ├── package.json
│   ├── .env.example            # Template environment variables
│   └── .env                    # Environment variables (local)
│
├── client/                      # Frontend (React + Vite)
│   ├── src/
│   │   ├── pages/              # React pages
│   │   ├── components/         # React components
│   │   ├── hooks/              # Custom React hooks
│   │   ├── stores/             # State management (Zustand)
│   │   ├── utils/              # Utility functions
│   │   ├── styles/             # CSS/Tailwind styles
│   │   └── main.jsx            # Entry point
│   ├── package.json
│   ├── vite.config.js          # Vite configuration
│   ├── tailwind.config.js      # Tailwind CSS config
│   └── .env.local              # Environment variables (local)
│
└── README.md                   # File này
```

## API Documentation

Hệ thống cung cấp **93 API endpoints** được phân loại thành 15 subsystems:

| Subsystem | Số Lượng API | Mô Tả |
|-----------|-------------|-------|
| Authentication & User | 11 | Login, Register, CRUD user |
| Category | 4 | Quản lý danh mục sản phẩm |
| Subcategory | 7 | Quản lý danh mục con |
| Brand | 4 | Quản lý thương hiệu |
| Material | 4 | Quản lý chất liệu |
| Gemstone | 4 | Quản lý đá quý |
| Product | 12 | CRUD sản phẩm, variant |
| Shopping Cart | 5 | Quản lý giỏ hàng |
| Order | 9 | Quản lý đơn hàng thường |
| Custom Order | 7 | Quản lý đơn hàng tùy chỉnh |
| Review | 5 | Quản lý đánh giá sản phẩm |
| Wishlist | 3 | Danh sách yêu thích |
| Coupon | 3 | Mã giảm giá |
| Payment | 4 | Tích hợp thanh toán PayOS |
| Others | 6 | Comparison, ChatBox, Provinces, File, Dashboard |

Chi tiết đầy đủ: Xem `CHAPTER_4_2_2_API_ENDPOINTS_LIST.md` và `CHAPTER_4_2_3_API_DETAILED_DESIGN.md`

## Tài Khoản Admin Mặc Định

Sau khi cài đặt xong, bạn cần tạo tài khoản admin:

1. Truy cập trang đăng ký
2. Tạo tài khoản thông thường
3. Mở MongoDB Compass hoặc MongoDB Atlas
4. Tìm user vừa tạo trong collection `users`
5. Cập nhật trường `role` từ `user` thành `admin`

## Tính Năng Chính của Server

### Dependencies Chính
- **Express.js**: Web framework
- **Mongoose**: MongoDB ORM
- **JWT & bcrypt**: Authentication & security
- **Cloudinary**: Cloud storage cho ảnh
- **Socket.io**: Real-time communication
- **PayOS**: Payment gateway
- **Nodemailer**: Email service
- **OpenAI**: AI chatbot
- **XLSX**: Excel file processing

### Middleware & Security
- **CORS**: Hỗ trợ cross-origin requests
- **Helmet**: Bảo mật HTTP headers
- **Rate Limiting**: Giới hạn số lượng requests
- **JWT Authentication**: Xác thực người dùng
- **Role-based Access Control**: Kiểm soát quyền truy cập

## Tính Năng Chính của Client

### Framework & Libraries
- **React 19**: UI framework
- **Vite**: Build tool (blazing fast)
- **React Router**: Navigation
- **Tailwind CSS**: Utility-first CSS
- **Material-UI**: Component library
- **Socket.io Client**: Real-time chat
- **SWR**: Data fetching & caching
- **Zustand**: State management
- **React Hook Form**: Form management
- **Zod**: Data validation

## Troubleshooting

### Lỗi: "Cannot connect to MongoDB"
- Kiểm tra MongoDB connection string trong `.env`
- Nếu dùng MongoDB Atlas, thêm IP của máy vào whitelist
- Kiểm tra MongoDB service đang chạy (nếu local)

### Lỗi: "CORS error"
- Kiểm tra `origin` trong `server.js` khớp với client URL
- Mặc định: `http://localhost:5173`

### Lỗi: "Cloudinary error"
- Kiểm tra credentials Cloudinary đúng trong `.env`
- Kiểm tra folder mặc định tồn tại trên Cloudinary

### Lỗi: "Port 3000 hoặc 5173 đã được sử dụng"
- Đóng các ứng dụng khác đang dùng port này
- Hoặc thay đổi port trong server.js/vite.config.js

### Lỗi: "npm install fail"
```bash
# Xóa node_modules và package-lock.json, rồi cài lại
rm -rf node_modules package-lock.json
npm install
```

## Development

### Chạy Development Mode

Đã hướng dẫn ở trên:
```bash
# Terminal 1 - Server
cd server && npm run dev

# Terminal 2 - Client
cd client && npm run dev
```

### Build Production

```bash
# Build Client
cd client
npm run build
# Output: client/dist/
```

### Linting

```bash
cd client
npm run lint
```

## Danh Sách Báo Cáo & Tài Liệu

- `CLASS_DIAGRAM_REPORT.md` - Biểu đồ lớp chi tiết (15 models)
- `CHAPTER_3_1_1_SUBSYSTEMS_AND_INTERFACES.md` - Xác định hệ thống con và giao diện
- `CHAPTER_4_2_1_API_STRUCTURE_OVERVIEW.md` - Cấu trúc API
- `CHAPTER_4_2_2_API_ENDPOINTS_LIST.md` - Danh sách 93 API endpoints
- `CHAPTER_4_2_3_API_DETAILED_DESIGN.md` - Thiết kế chi tiết API

## Thông Tin Liên Hệ & Support

- **GitHub Repository**: https://github.com/lionelmahn/WEB-JEWELRY
- **Issues**: Báo cáo lỗi tại GitHub Issues

## License

Dự án này được phân phối dưới giấy phép ISC.

## Ghi Chú

- Dự án sử dụng **ES6 modules** (`type: "module"` trong package.json)
- Tất cả routes được prefix với `/api`
- Socket.io được cấu hình để kết nối từ `http://localhost:5173`
- Tất cả requests cần JWT token trong header (ngoại trừ login/register)
- File `.env` không được commit lên GitHub (thêm vào .gitignore)

---

Chúc bạn cài đặt và phát triển thành công! Nếu có bất kỳ vấn đề nào, vui lòng tạo issue trên GitHub.
