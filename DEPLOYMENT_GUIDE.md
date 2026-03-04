# Hướng Dẫn Deployment Jewelry System

## Lỗi Deployment và Cách Sửa

### Lỗi: "No Output Directory named 'build' found"

**Nguyên nhân**: Vercel đang tìm kiếm thư mục `build/` nhưng Vite tạo ra `dist/`

**Giải pháp**: Đã tạo/cập nhật 2 file:

1. **vercel.json** (tại root project)
   - Cấu hình Vercel để chỉ định output directory là `client/dist`
   - Build command được cấu hình đúng

2. **vite.config.js** (tại client/)
   - Cấu hình Vite build output directory
   - Tối ưu chunk size và code splitting
   - Cấu hình proxy API cho development

---

## Deployment lên Vercel

### Prerequisites
- GitHub account đã push code
- Vercel account (có thể đăng nhập bằng GitHub)

### Bước 1: Kết Nối GitHub Repository

1. Truy cập https://vercel.com
2. Click "Add New..." → "Project"
3. Chọn repository `WEB-JEWELRY`
4. Vercel sẽ tự động detect Vite project

### Bước 2: Cấu Hình Environment Variables

Tại trang cấu hình Project, thêm các biến môi trường:

```
VITE_API_URL=<your-backend-url>
```

**Ví dụ**:
- Development: `http://localhost:3000`
- Production: `https://jewelry-api.onrender.com` (nếu backend deployed ở Render/Railway)

### Bước 3: Deploy

Sau khi cấu hình xong:
1. Click "Deploy"
2. Vercel sẽ tự động build và deploy
3. Sau 2-3 phút, dự án sẽ available tại URL như: `https://web-jewelry.vercel.app`

---

## Deployment Backend (Node.js + MongoDB)

Có 2 lựa chọn:

### Option 1: Render.com (Khuyên dùng)

**Bước 1: Chuẩn bị**
- Push code server lên GitHub
- Tạo file `start` script trong `server/package.json`:

```json
{
  "scripts": {
    "start": "node src/server.js",
    "dev": "nodemon src/server.js"
  }
}
```

**Bước 2: Deploy trên Render**
1. Truy cập https://render.com
2. Click "New+" → "Web Service"
3. Kết nối GitHub repository
4. Cấu hình:
   - Runtime: Node
   - Build Command: `npm install`
   - Start Command: `npm start`
   - Environment: Production

**Bước 3: Thêm Environment Variables**
- MONGO_URL: MongoDB Atlas connection string
- JWT_SECRET: Secret key cho JWT
- CLOUDINARY_*: Credentials
- PAYOS_*: Payment API keys
- Các biến khác (theo .env.example)

**Bước 4: Deploy**
- Click "Create Web Service"
- Chờ 3-5 phút để build xong
- Lấy URL backend (ví dụ: `https://jewelry-api.onrender.com`)

### Option 2: Railway.app

1. Truy cập https://railway.app
2. Click "New Project"
3. Chọn GitHub repository
4. Railway sẽ auto-detect Node.js project
5. Thêm MongoDB (Railway có plugin)
6. Cấu hình environment variables
7. Auto-deploy

---

## Cấu Hình Database

### MongoDB Atlas (Recommended)

1. Tạo account tại https://www.mongodb.com/cloud/atlas
2. Tạo Cluster (free tier available)
3. Lấy connection string: `mongodb+srv://username:password@cluster0.mongodb.net/jewelry?retryWrites=true&w=majority`
4. Thêm vào MONGO_URL ở backend environment variables

**Security**:
- Thêm IP Whitelist (hoặc `0.0.0.0/0` cho testing)
- Tạo user riêng (không dùng root account)
- Dùng strong password

---

## Tối Ưu Performance

### Client-side Optimization
- Vite đã được cấu hình code splitting tối ưu
- Chunks được chia theo: vendor, ui-components, utils
- Asset optimization: image compression, lazy loading

### Server-side Optimization
Thêm vào `server/src/server.js`:

```javascript
// Compression middleware
import compression from 'compression';
app.use(compression());

// Rate limiting
import rateLimit from 'express-rate-limit';
const limiter = rateLimit({
  windowMs: 15 * 60 * 1000, // 15 minutes
  max: 100 // limit each IP to 100 requests per windowMs
});
app.use('/api/', limiter);
```

---

## Monitoring & Logging

### Vercel Analytics
- Tự động tracking performance
- Xem tại Vercel Dashboard → Analytics

### Backend Logging
Cấu hình logging cho production:

```javascript
// Sử dụng winston hoặc morgan
import morgan from 'morgan';
app.use(morgan('combined'));
```

---

## Troubleshooting Deployment

### Lỗi: "CORS error in production"

**Giải pháp**: Cập nhật CORS trong `server/src/server.js`:

```javascript
app.use(cors({
  origin: [
    'http://localhost:5173',
    'https://web-jewelry.vercel.app', // Frontend URL
  ],
  credentials: true
}));
```

### Lỗi: "API requests timeout"

**Giải pháp**: 
- Tăng timeout trong `vite.config.js` proxy
- Kiểm tra backend service đang chạy
- Tăng memory limit trên hosting

### Lỗi: "Cannot upload images"

**Giải pháp**:
- Kiểm tra Cloudinary credentials
- Kiểm tra file size limit (max 25MB)
- Đảm bảo folder name đúng trong Cloudinary

### Lỗi: "Database connection refused"

**Giải pháp**:
- Kiểm tra MongoDB Atlas whitelist (add your IP)
- Kiểm tra connection string đúng
- Kiểm tra network connectivity

---

## Continuous Deployment

### Auto-deploy từ GitHub

**Vercel**: Tự động deploy khi push lên main branch
**Render**: Tự động deploy khi push lên main branch

Cấu hình ở Project Settings → "Deploy on Push"

### Manual Deploy

```bash
# Deploy frontend
vercel --prod

# Deploy backend (nếu dùng Vercel Functions)
vercel --prod
```

---

## Danh Sách Checklist Deployment

- [ ] Cấu hình `vercel.json` (✓ đã tạo)
- [ ] Cấu hình `vite.config.js` (✓ đã update)
- [ ] Tạo MongoDB Atlas database
- [ ] Tạo/cấu hình Cloudinary account
- [ ] Cấu hình PayOS (nếu cần thanh toán)
- [ ] Push code lên GitHub
- [ ] Connect Vercel với GitHub repo
- [ ] Thêm environment variables trên Vercel
- [ ] Deploy backend (Render/Railway)
- [ ] Thêm VITE_API_URL pointing tới backend
- [ ] Test APIs từ production environment
- [ ] Enable HTTPS (tự động từ Vercel)
- [ ] Setup monitoring & logging

---

## Domain Custom

### Thêm Domain Custom trên Vercel

1. Vercel Dashboard → Project → Settings → Domains
2. Thêm domain
3. Update DNS records (A record hoặc CNAME) theo hướng dẫn
4. Chờ propagation (15 phút - 48 giờ)

---

Chúc bạn deployment thành công! 🚀
