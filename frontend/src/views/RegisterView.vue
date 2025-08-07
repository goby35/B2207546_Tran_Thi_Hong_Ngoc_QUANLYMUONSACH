<template>
  <div class="container d-flex flex-column justify-content-center align-items-center vh-50 mt-5">
    <div class="card p-3 shadow-sm w-100" style="max-width: 400px;">
      <h2 class="text-center mb-3">Đăng ký</h2>
      <form @submit.prevent="handleRegister">
        <div class="mb-3">
          <input type="text" class="form-control" v-model="sdt" placeholder="Số điện thoại" required />
        </div>
        <div class="mb-3">
          <input type="password" class="form-control" v-model="password" placeholder="Mật khẩu" required />
        </div>
        <div class="mb-3">
          <input type="password" class="form-control" v-model="confirmPassword" placeholder="Xác nhận mật khẩu" required />
        </div>
        <button type="submit" class="btn btn-primary w-100">Đăng ký</button>
      </form>
      <div class="text-center mt-3">
        <p class="mb-0">Bạn đã có tài khoản? 
          <router-link to="/login-docgia" class="text-decoration-none"><span>Đăng nhập</span></router-link>
        </p>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      sdt: '',
      password: '',
      confirmPassword: '',
    };
  },
  methods: {
    async handleRegister() {
      if (this.password !== this.confirmPassword) {
        alert('Mật khẩu không khớp!');
        return;
      }

      try {
        await axios.post('http://localhost:3000/api/auth/register/docgia', { 
          sdt: this.sdt, 
          password: this.password, 
          confirmPassword: this.confirmPassword });
        alert('Đăng ký thành công');
        this.$router.push('/login-docgia');
      } catch (error) {
        alert(error.response?.data?.message || 'Đăng ký thất bại');
      }
    }
  }
};
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap');

h2 {
  color: #1E3A8A;
  font-family: 'Inter', sans-serif;
  font-size: 24px;
  font-weight: 600;
  margin-bottom: 20px;
}

button {
  background-color: #1E3A8A;
  color: #FFFFFF;
  border: none;
  font-family: 'Inter', sans-serif;
  font-size: 14px;
  font-weight: 500;
  transition: background-color 0.2s ease-in-out;
}

button:hover {
  background-color: #1E40AF;
}

span {
  color: #1E3A8A;
  font-family: 'Inter', sans-serif;
  font-size: 14px;
  font-weight: 500;
}

.card {
  background-color: #FFFFFF;
  border-radius: 12px;
  padding: 20px;
}
</style>