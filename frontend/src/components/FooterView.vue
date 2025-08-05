<template>
  <footer class="container-fluid footer">
    <div class="d-flex flex-column flex-lg-row justify-content-between gap-4">
      <div class="footer-info ps-lg-5">
        <h2 class="fw-bold mb-0">Hệ Thống Quản Lý Mượn Sách</h2>
        <p class="text-muted">
          Đăng ký và quản lý mượn sách online 24/7.
        </p>
      </div>
    </div>

    <div class="container mx-auto px-0 px-lg-1">
      <div class="row">
        <div class="col-4 ps-lg-5 text-center text-lg-start">
          <div class="fw-bold fs-4">
            Chi Tiết
            <span class="text-primary-custom">Liên Hệ</span>
          </div>
          <div class="text-muted my-3">
            <i class="bi bi-geo-alt"></i>
            Đại học Cần Thơ
          </div>
          <div class="text-muted my-3">
            <i class="bi bi-envelope"></i>
            Công Nghệ Thông Tin và Truyền Thông
          </div>
        </div>

        <div class="col-4 ps-lg-5 text-center text-lg-start">
          <b>Thông Tin</b>
          <div class="footer-links">
            <router-link to="/gioi-thieu" class="footer-link">
              Giới thiệu
            </router-link>
            <a
              href="#"
              class="footer-link"
              @click.prevent="showDieuKhoan = true"
            >
              Điều khoản mượn sách
            </a>
            <a
              href="#"
              class="footer-link"
              @click.prevent="showHuongDan = true"
            >
              Hướng dẫn sử dụng
            </a>
            <router-link to="/lien-he" class="footer-link">Liên hệ</router-link>
          </div>
        </div>

        <div class="col-4 ps-lg-5 text-center text-lg-start">
          <b>Social</b>
          <div class="footer-links">
            <a
              href="/"
              target="_blank"
              rel="noopener noreferrer"
              class="footer-link"
            >
              <i class="bi bi-facebook"></i>
              Facebook
            </a>
            <a
              href="/"
              target="_blank"
              rel="noopener noreferrer"
              class="footer-link"
            >
              <i class="bi bi-instagram"></i>
              Instagram
            </a>
          </div>
        </div>
      </div>
    </div>

    <div class="footer-bottom text-center">
      <div class="text-muted">Bản quyền © 2025.</div>
    </div>

    <div
      class="modal fade show"
      v-if="showDieuKhoan"
      style="display: block"
      aria-modal="true"
    >
      <div class="modal-dialog modal-lg">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title">Điều khoản mượn sách</h5>
            <button
              type="button"
              class="btn-close"
              @click="showDieuKhoan = false"
            ></button>
          </div>
          <div class="modal-body">
            <p>
              <i class="bi bi-bookmark"></i>
              Độc giả cần có tài khoản hợp lệ để mượn sách.
            </p>
            <p>
              <i class="bi bi-bookmark"></i>
              Thời gian mượn sách tối đa là 20 ngày.
            </p>
            <p>
              <i class="bi bi-bookmark"></i>
              Quá hạn sẽ bị phạt theo quy định.
            </p>
            <p>
              <i class="bi bi-bookmark"></i>
              Nếu mất hoặc làm hỏng sách, độc giả phải bồi thường.
            </p>
          </div>
          <div class="modal-footer">
            <button class="btn btn-secondary" @click="showDieuKhoan = false">
              Đóng
            </button>
          </div>
        </div>
      </div>
    </div>

    <div
      class="modal fade show"
      v-if="showHuongDan"
      style="display: block"
      aria-modal="true"
    >
      <div class="modal-dialog modal-lg">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title">Hướng dẫn sử dụng</h5>
            <button
              type="button"
              class="btn-close"
              @click="showHuongDan = false"
            ></button>
          </div>
          <div class="modal-body">
            <p>
              <i class="bi bi-1-circle"></i>
              <b>Đăng nhập/Đăng ký:</b>
              Sử dụng số điện thoại để đăng nhập hoặc đăng ký tài khoản.
            </p>
            <p>
              <i class="bi bi-2-circle"></i>
              <b>Tìm kiếm sách:</b>
              Vào trang chủ để xem danh mục sách hoặc tìm sách với thanh tìm
              kiếm.
            </p>
            <p>
              <i class="bi bi-3-circle"></i>
              <b>Đăng ký mượn:</b>
              Chọn sách, thêm vào danh sách mượn, sau đó xác nhận.
            </p>
            <p>
              <i class="bi bi-4-circle"></i>
              <b>Kiểm tra lịch sử:</b>
              Xem trạng thái mượn sách trong mục
              <b>"Lịch sử mượn"</b>
              .
            </p>
            <p>
              <i class="bi bi-5-circle"></i>
              <b>Trả sách:</b>
              Mang sách đến thư viện hoặc sử dụng dịch vụ giao nhận để gửi lại.
            </p>
          </div>
          <div class="modal-footer">
            <button class="btn btn-secondary" @click="showHuongDan = false">
              Đóng
            </button>
          </div>
        </div>
      </div>
    </div>

    <div
      class="modal-backdrop fade show"
      v-if="showDieuKhoan || showHuongDan"
    ></div>
  </footer>
</template>

<script>
export default {
  data() {
    return {
      showDieuKhoan: false,
      showHuongDan: false
    };
  }
};
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap');

.footer {
  background-color: #1E3A8A;
  padding: 24px 0;
  color: #FFFFFF;
  font-family: 'Inter', sans-serif;
  box-shadow: 0 -2px 4px rgba(0, 0, 0, 0.1);
}

.footer-info h2 {
  font-size: 20px;
  font-weight: 600;
  color: #FFFFFF;
  margin-bottom: 8px;
}

.footer-info p {
  font-size: 14px;
  color: #D1D5DB;
}

.text-muted {
  font-size: 14px;
  color: #D1D5DB;
}

.footer-links {
  display: flex;
  flex-direction: column;
  gap: 10px;
}

.footer-link {
  font-size: 14px;
  font-weight: 500;
  color: #D1D5DB;
  text-decoration: none;
  transition: color 0.2s;
}

.footer-link:hover {
  color: #60A5FA;
}

.footer-bottom {
  text-align: center;
  padding: 12px 0;
  border-top: 1px solid #4B5EAA;
  margin-top: 24px;
  font-size: 14px;
  color: #D1D5DB;
}

.modal-content {
  border-radius: 8px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.2);
  background-color: #FFFFFF;
}

.modal-header {
  background-color: #F3F4F6;
  border-bottom: 1px solid #D1D5DB;
  padding: 16px;
}

.modal-title {
  font-family: 'Inter', sans-serif;
  font-size: 20px;
  font-weight: 600;
  color: #1E3A8A;
}

.btn-close {
  font-size: 16px;
  transition: opacity 0.2s;
}

.btn-close:hover {
  opacity: 0.7;
}

.modal-body {
  padding: 20px;
  font-family: 'Inter', sans-serif;
  font-size: 14px;
  color: #374151;
}

.modal-body p {
  margin-bottom: 12px;
}

.modal-footer {
  border-top: 1px solid #D1D5DB;
  padding: 12px;
}

.btn-secondary {
  background-color: #6B7280;
  border: none;
  padding: 8px 16px;
  border-radius: 6px;
  font-family: 'Inter', sans-serif;
  font-size: 14px;
  font-weight: 500;
  color: #FFFFFF;
  transition: background-color 0.2s;
}

.btn-secondary:hover {
  background-color: #4B5563;
}

.modal-backdrop {
  background-color: rgba(0, 0, 0, 0.5);
}

@media (max-width: 768px) {
  .footer {
    padding: 20px 0;
  }

  .footer .d-flex {
    flex-direction: column;
    gap: 20px;
  }

  .footer-info h2 {
    font-size: 18px;
  }

  .footer-links {
    align-items: center;
  }

  .footer-link {
    font-size: 14px;
  }
}

@media (max-width: 480px) {
  .footer-info h2 {
    font-size: 16px;
  }

  .footer-info p {
    font-size: 13px;
  }

  .footer-bottom {
    font-size: 13px;
  }
}
</style>