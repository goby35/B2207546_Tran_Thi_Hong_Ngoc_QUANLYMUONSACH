<template>
  <div
    class="container d-flex justify-content-center align-items-center vh-50 mt-5"
  >
    <div class="card p-3 shadow-sm" style="max-width: 400px; width: 100%">
      <h2 class="text-center">Đăng nhập nhân viên</h2>
      <form @submit.prevent="handleLogin">
        <div class="mb-3">
          <label for="sdt" class="form-label">Số điện thoại</label>
          <input
            type="text"
            id="sdt"
            v-model="sdt"
            class="form-control"
            placeholder="Nhập số điện thoại"
            required
          />
        </div>
        <div class="mb-3">
          <label for="password" class="form-label">Mật khẩu</label>
          <input
            type="password"
            id="password"
            v-model="password"
            class="form-control"
            placeholder="Nhập mật khẩu"
            required
          />
        </div>
        <button type="submit" class="btn btn-primary w-100">Đăng nhập</button>
      </form>
      <div class="text-start mt-2">
        <router-link to="/login-docgia" class="text-decoration-none text-muted">
          Bạn là độc giả?
        </router-link>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  data() {
    return {
      sdt: '',
      password: ''
    }
  },
  methods: {
    async handleLogin() {
      try {
        const response = await axios.post(
          'http://localhost:3000/api/auth/login/nhanvien',
          {
            sdt: this.sdt,
            password: this.password
          }
        )

        const token = response.data?.token || response.data?.user?.token
        const chucVu = response.data?.CHUCVU || response.data?.user?.CHUCVU
        const id = response.data?._id || response.data?.user?._id

        let role = 'docgia'
        if (chucVu === 'QuanLyThuVien') {
          role = 'quanly'
        } else if (chucVu) {
          role = 'nhanvien'
        }

        this.$store.dispatch('login', { _id: id, role: role, token: token })

        alert('Đăng nhập nhân viên thành công')
        this.$router.push('/')
      } catch (error) {
        if (error.response) {
          if (
            error.response.status === 404 ||
            error.response.status === 500
          ) {
            alert('Số điện thoại chưa được đăng ký!')
          } else if (error.response.status === 401) {
            alert('Mật khẩu không đúng!')
          } else {
            alert('Lỗi hệ thống, vui lòng thử lại!')
          }
        } else {
          alert('Không thể kết nối đến máy chủ!')
        }
      }
    }
  }
}
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap');

h2 {
  font-family: 'Inter', sans-serif;
  font-size: 24px;
  color: #1E3A8A;
  margin-bottom: 20px;
  font-weight: 600;
}

button {
  background-color: #1E3A8A;
  color: #FFFFFF;
  border: none;
  font-family: 'Inter', sans-serif;
  font-size: 14px;
  font-weight: 500;
  padding: 10px;
  border-radius: 8px;
  transition: background-color 0.2s ease-in-out;
}

button:hover {
  background-color: #1E40AF;
}

.card {
  background-color: #FFFFFF;
  border: 1px solid #E5E7EB;
  border-radius: 12px;
}

.form-control {
  border: 2px solid #D1D5DB;
  border-radius: 8px;
  font-family: 'Inter', sans-serif;
  font-size: 14px;
  padding: 10px;
}

.form-control:focus {
  border-color: #9CA3AF;
  box-shadow: none;
}

.form-label {
  font-family: 'Inter', sans-serif;
  font-size: 14px;
  color: #374151;
  font-weight: 500;
}
</style>