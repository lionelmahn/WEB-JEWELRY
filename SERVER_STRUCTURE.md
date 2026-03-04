# Cấu Trúc Thư Mục Server

## Mô Tả Các Folder Chính

| STT | Thành phần | Mô tả |
|-----|-----------|-------|
| 1. | **AI** | Thư mục chứa các file liên quan đến tích hợp AI và xử lý logic về trí tuệ nhân tạo. |
| 2. | **auth** | Thư mục chứa các file xác thực và cấp quyền người dùng (login, register, JWT tokens, etc.). |
| 3. | **config** | Thư mục cấu hình ứng dụng bao gồm cài đặt database, email, SMS, token, và các biến môi trường. |
| 4. | **controller** | Thư mục chứa các file xử lý logic chính cho các chức năng (Auth, Disease, HealthOrganization, Account, etc.). |
| 5. | **core** | Thư mục chứa các hàm lõi, utility cơ bản được sử dụng chung trong toàn bộ ứng dụng. |
| 6. | **libs** | Thư mục chứa các thư viện hỗ trợ, helper functions, và các công cụ tiện ích khác. |
| 7. | **middleware** | Thư mục chứa các middleware xử lý request/response (authentication, validation, error handling, etc.). |
| 8. | **models** | Thư mục định nghĩa các schema/model dữ liệu để giao tiếp với database MongoDB. |
| 9. | **routes** | Thư mục định tuyến API, quản lý các endpoint và kết nối với các controller tương ứng. |
| 10. | **Schema** | Thư mục chứa các schema validation để kiểm tra dữ liệu request. |
| 11. | **services** | Thư mục chứa các business logic, xử lý dữ liệu và gọi database (separation of concerns). |
| 12. | **uploads** | Thư mục lưu trữ các file được upload từ client (hình ảnh, tài liệu, etc.). |

---

## Cấu Trúc Chi Tiết

```
server/
├── node_modules/        # Các package được install qua npm/yarn
├── src/
│   ├── AI/             # Tích hợp AI
│   ├── auth/           # Xác thực người dùng
│   ├── config/         # Cấu hình ứng dụng
│   ├── controller/     # Xử lý logic chính
│   ├── core/           # Hàm lõi và utility chung
│   ├── libs/           # Thư viện hỗ trợ
│   ├── middleware/     # Middleware
│   ├── models/         # Schema/Model database
│   ├── routes/         # API routes
│   ├── Schema/         # Schema validation
│   ├── services/       # Business logic
│   ├── uploads/        # Lưu trữ upload files
│   ├── server.js       # File entry point
│   ├── swagger.js      # Cấu hình Swagger/OpenAPI
│   └── test.js         # Test scripts
├── uploads/            # Folder lưu file upload tại root
├── .env               # Biến môi trường
├── .env.example       # Ví dụ biến môi trường
├── .gitignore         # Git ignore rules
├── package-lock.json  # Lock file dependencies
├── package.json       # Project dependencies
└── README.md          # Tài liệu dự án
```

---

## Quy Ước Cấu Trúc

- **src/**: Tất cả mã nguồn chính được tổ chức theo tính năng và layer
- **server.js**: Entry point của ứng dụng server
- **swagger.js**: Cấu hình API documentation
- **uploads/**: Thư mục lưu file upload từ người dùng
- **.env**: File cấu hình biến môi trường (không commit lên git)
