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
  </footer>
</template>

<script>
export default {
  data() {
    return {};
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

@media (max-width: 768px) {
  .footer {
    padding: 20px 0;
  }

  .footer-info h2 {
    font-size: 18px;
  }
}

@media (max-width: 480px) {
  .footer-info h2 {
    font-size: 16px;
  }

  .footer-info p {
    font-size: 13px;
  }
}
</style>