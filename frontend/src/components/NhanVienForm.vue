<template>
  <form @submit.prevent="SubmitForm">
    <div class="form-group">
      <label for="HOTENNV">Họ tên nhân viên:</label>
      <input
        type="text"
        class="form-control"
        v-model="nhanvienLocal.HOTENNV"
        placeholder="Nhập họ tên nhân viên"
        required
      />
    </div>
    <div class="form-group">
      <label for="SODIENTHOAI">Số điện thoại:</label>
      <input
        type="text"
        class="form-control"
        v-model="nhanvienLocal.SODIENTHOAI"
        placeholder="Nhập số điện thoại"
        required
      />
    </div>
    <div class="form-group">
      <label for="PASSWORD">Mật khẩu:</label>
      <input
        type="password"
        class="form-control"
        v-model="nhanvienLocal.PASSWORD"
        placeholder="Nhập mật khẩu"
        required
      />
    </div>
    <div class="radio-group">
      <label>
        <input
          type="radio"
          value="QuanLyThuVien"
          v-model="nhanvienLocal.CHUCVU"
        />
        Quản lý
      </label>
      <label>
        <input
          type="radio"
          value="NhanVienThuVien"
          v-model="nhanvienLocal.CHUCVU"
        />
        Nhân viên
      </label>
    </div>
    <div class="button-group">
      <button type="button" class="btn btn-success" @click="SubmitForm">
        Lưu
      </button>
      <button
        type="button"
        class="btn btn-secondary ml-2"
        @click="$emit('cancel')"
      >
        Hủy
      </button>
    </div>
  </form>
</template>

<script>
export default {
  props: {
    nhanvien: {
      type: Object,
      default: () => ({
        HOTENNV: '',
        SODIENTHOAI: '',
        PASSWORD: '',
        CHUCVU: ''
      })
    }
  },
  data() {
    return {
      nhanvienLocal: { ...this.nhanvien },
      hasChanged: false
    }
  },
  watch: {
    nhanvienLocal: {
      handler() {
        this.hasChanged = true
      },
      deep: true
    }
  },
  methods: {
    SubmitForm() {
      if (!this.hasChanged) return
      if (!this.nhanvienLocal.HOTENNV.trim()) {
        alert('Họ tên nhân viên không được để trống!')
        return
      }
      if (!this.nhanvienLocal.SODIENTHOAI.trim()) {
        alert('Số điện thoại không được để trống!')
        return
      }
      if (!this.nhanvienLocal.PASSWORD.trim()) {
        alert('Mật khẩu không được để trống!')
        return
      }
      if (!this.nhanvienLocal.CHUCVU.trim()) {
        alert('Chức vụ không được để trống!')
        return
      }
      this.$emit('submit', this.nhanvienLocal)
      this.hasChanged = false
    }
  }
}
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap');

form {
  max-width: 500px;
  margin: 20px auto;
  padding: 20px;
  background: #FFFFFF;
  border-radius: 12px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
}

.form-group {
  margin-bottom: 15px;
}

label {
  font-weight: 500;
  display: block;
  margin-bottom: 5px;
  font-family: 'Inter', sans-serif;
  color: #374151;
}

input {
  width: 100%;
  padding: 10px;
  border: 1px solid #D1D5DB;
  border-radius: 6px;
  font-size: 14px;
  font-family: 'Inter', sans-serif;
}

input:focus {
  border-color: #1E3A8A;
  outline: none;
  box-shadow: 0 0 5px rgba(30, 58, 138, 0.5);
}

.radio-group {
  display: flex;
  gap: 15px;
  margin-bottom: 15px;
}

.radio-group label {
  display: flex;
  align-items: center;
  gap: 8px;
  font-size: 14px;
  cursor: pointer;
  padding: 8px 12px;
  border: 1px solid #D1D5DB;
  border-radius: 6px;
  transition: all 0.2s ease-in-out;
  font-family: 'Inter', sans-serif;
  color: #374151;
}

.radio-group input {
  width: auto;
  margin: 0;
  accent-color: #1E3A8A;
}

.radio-group label:hover {
  background-color: #F3F4F6;
  border-color: #1E3A8A;
}

.radio-group input:checked + label {
  background-color: #1E3A8A;
  color: #FFFFFF;
  border-color: #1E40AF;
}

.button-group {
  display: flex;
  justify-content: space-between;
  gap: 15px;
  margin-top: 20px;
}

.btn {
  padding: 10px 15px;
  border-radius: 6px;
  font-size: 14px;
  cursor: pointer;
  transition: background-color 0.2s ease-in-out;
  width: 120px;
  text-align: center;
  font-family: 'Inter', sans-serif;
  font-weight: 500;
}

.btn-success {
  background-color: #28A745;
  border: none;
  color: #FFFFFF;
}

.btn-success:hover {
  background-color: #218838;
}

.btn-secondary {
  background-color: #6C757D;
  border: none;
  color: #FFFFFF;
}

.btn-secondary:hover {
  background-color: #5A6268;
}
</style>