<template>
  <nav class="navbar navbar-expand-lg">
    <div class="container">
      <div class="navbar-brand">Quản lý Mượn Sách</div>
      <div class="collapse navbar-collapse" id="navbarNav">
        <ul class="navbar-nav ms-auto">
          <li class="nav-item">
            <router-link class="nav-link" to="/">Trang chủ</router-link>
          </li>

          <template v-if="userRole === 'docgia'">
            <li class="nav-item">
              <router-link class="nav-link" to="/muon-sach">
                Đăng Ký Mượn Sách
              </router-link>
            </li>
            <li class="nav-item">
              <router-link class="nav-link" to="/lich-su-muon">
                Lịch Sử Mượn
              </router-link>
            </li>
            <li class="nav-item">
              <router-link class="nav-link" to="/tai-khoan">
                Tài Khoản
              </router-link>
            </li>
          </template>

          <template v-else-if="userRole === 'quanly'">
            <li class="nav-item">
              <router-link class="nav-link" to="/quan-ly-sach">
                Quản Lý Sách
              </router-link>
            </li>
            <li class="nav-item">
              <router-link class="nav-link" to="/quan-ly-tai-khoan">
                Quản Lý Tài Khoản
              </router-link>
            </li>
            <li class="nav-item">
              <router-link class="nav-link" to="/theo-doi-muon">
                Quản Lý Mượn Sách
              </router-link>
            </li>
            <li class="nav-item">
              <router-link class="nav-link" to="/tai-khoan">
                Tài Khoản
              </router-link>
            </li>
          </template>

          <template v-else-if="userRole === 'nhanvien'">
            <li class="nav-item">
              <router-link class="nav-link" to="/theo-doi-muon">
                Quản Lý Mượn Sách
              </router-link>
            </li>
            <li class="nav-item">
              <router-link class="nav-link" to="/tai-khoan">
                Tài Khoản
              </router-link>
            </li>
          </template>

          <template v-else>
            <li class="nav-item">
              <router-link class="nav-link" to="/login-docgia">
                Đăng nhập
              </router-link>
            </li>
            <li class="nav-item">
              <router-link class="nav-link" to="/register">
                Đăng ký
              </router-link>
            </li>
          </template>

          <li class="nav-item" v-if="userRole">
            <router-link class="nav-link logout-btn" to="/logout" @click="handleLogout">
              Đăng Xuất
            </router-link>
          </li>
        </ul>
      </div>
    </div>
  </nav>
</template>

<script>
export default {
  computed: {
    userRole() {
      return this.$store.state.user.role;
    }
  },
  methods: {
    handleLogout() {
      this.$store.dispatch('logout');
      this.$router.push('/login-docgia');
    }
  }
};
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap');

.navbar {
  background-color: #1E3A8A;
  padding: 12px 0;
  border-radius: 0 0 8px 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.navbar-brand {
  font-family: 'Inter', sans-serif;
  font-size: 20px;
  font-weight: 600;
  color: #FFFFFF;
  margin-right: 24px;
}

.navbar-nav {
  display: flex;
  justify-content: flex-end;
}

.nav-link {
  font-family: 'Inter', sans-serif;
  font-size: 16px;
  font-weight: 500;
  color: #FFFFFF !important;
  padding: 8px 16px;
  margin: 0 8px;
  border-radius: 6px;
  transition: background-color 0.2s;
}

.nav-link:hover {
  background-color: #1E40AF;
}

.nav-link.logout-btn {
  background-color: #EF4444;
  color: #FFFFFF !important;
}

.nav-link.logout-btn:hover {
  background-color: #DC2626;
}

@media (max-width: 992px) {
  .navbar-nav {
    text-align: center;
    padding-top: 12px;
  }

  .nav-link {
    display: block;
    margin: 8px 0;
    padding: 10px;
  }

  .navbar-brand {
    margin-right: 0;
    margin-bottom: 12px;
  }
}
</style>