# 📚 Quản Lý Mượn Sách Trực Tuyến

Hệ thống quản lý mượn sách được phát triển nhằm số hóa quy trình mượn – trả sách tại thư viện, giúp giảm thiểu thời gian, công sức cho thủ thư và người đọc, đồng thời nâng cao hiệu quả quản lý.

## 🔧 Công nghệ sử dụng

- **Frontend**: Vue.js
- **Backend**: Node.js + Express.js
- **Cơ sở dữ liệu**: MongoDB
- **Thư viện UI**: Bootstrap
- **Công cụ phát triển**: Visual Studio Code

## ⚙️ Chức năng chính

### 👤 Độc giả (User)
- Đăng ký, đăng nhập
- Tra cứu sách theo tên hoặc thể loại
- Gửi yêu cầu mượn sách
- Xem lịch sử mượn trả
- Cập nhật thông tin cá nhân

### 👨‍💼 Nhân viên thư viện
- Xem và duyệt yêu cầu mượn sách
- Cập nhật trạng thái trả sách

### 🧑‍💼 Quản lý thư viện
- Quản lý sách: thêm, sửa, xóa
- Quản lý tài khoản độc giả
- Theo dõi giao dịch mượn trả

## 📁 Cấu trúc thư mục

### Chạy dự án
## Khởi tạo dữ liệu 
cd backend
npm install
node seedSach.js

## Backend
npm start

## Frontend
cd ../frontend
npm install
npm run dev
