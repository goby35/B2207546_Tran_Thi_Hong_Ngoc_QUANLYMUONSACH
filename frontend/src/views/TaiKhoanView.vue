<template>
  <div class="account-view">
    <h1>Thông Tin Tài Khoản</h1>

    <TaiKhoanForm
      v-if="isEditing"
      :user="userInfo"
      :role="userRole"
      :userId="userInfo._id"
      @update="fetchUser"
      @cancel="isEditing = false"
    />
    <div class="avatar">
      {{ initials }}
    </div>
    <div v-if="userInfo">
      <p>
        <strong>Số điện thoại:</strong>
        {{ userInfo.sdt }}
      </p>
      <p>
        <strong>Họ và Tên:</strong>
        {{ userInfo.hoTen }}
      </p>
      <p v-if="userRole === 'docgia'">
        <strong>Giới tính:</strong>
        {{ userInfo.gioiTinh }}
      </p>
      <p v-if="userRole === 'docgia'">
        <strong>Ngày sinh:</strong>
        {{ formatDate(userInfo.ngaySinh) }}
      </p>
      <p>
        <strong>Quyền hạn:</strong>
        {{ userInfo.chucVu }}
      </p>
      <p>
        <strong>Địa chỉ:</strong>
        {{ userInfo.diaChi }}
      </p>
      <button class="mt-3" @click="isEditing = true">Chỉnh sửa</button>
    </div>

    <p v-else>Đang tải thông tin...</p>
  </div>
</template>

<script>
import { mapState } from 'vuex'
import { getUserInfo } from '@/services/accService'
import TaiKhoanForm from '@/components/TaiKhoanForm.vue'

export default {
  components: { TaiKhoanForm },
  data() {
    return {
      userInfo: null,
      isEditing: false
    }
  },
  computed: {
    ...mapState({
      userRole() {
        return this.$store.state.user.role
      },
      userID() {
        return this.$store.state.user._id
      }
    }),
    initials() {
      if (!this.userInfo || !this.userInfo.hoTen) return '?'
      const words = this.userInfo.hoTen.trim().split(' ')
      const lastLetter =
        words.length > 1
          ? words[words.length - 1].charAt(0).toUpperCase()
          : ''
      return lastLetter
    }
  },
  methods: {
    async fetchUser() {
      if (!this.userID) {
        console.error('Không tìm tài khoản!')
        return
      }
      try {
        const userData = await getUserInfo(this.userID, this.userRole)
        this.userInfo = {
          _id: userData._id,
          sdt: userData.SODIENTHOAI || 'Không có số điện thoại',
          diaChi: userData.DIACHI || 'Chưa cập nhật',
          role: this.userRole,
          hoTen:
            this.userRole === 'docgia'
              ? `${userData.HOLOT || ''} ${userData.TEN || ''}`
              : userData.HOTENNV || '',
          hoLot: userData.HOLOT || '',
          ten: userData.TEN || '',
          gioiTinh: userData.PHAI || '',
          chucVu:
            this.userRole === 'docgia'
              ? 'Độc giả'
              : userData.CHUCVU === 'QuanLyThuVien'
              ? 'Quản lý'
              : 'Nhân viên',
          ngaySinh:
            this.userRole === 'docgia' && userData?.NGAYSINH
              ? userData.NGAYSINH.split('T')[0]
              : ''
        }
      } catch (error) {
        console.error('Lỗi khi lấy thông tin tài khoản:', error)
      }
    },
    formatDate(dateString) {
      if (!dateString) return 'Chưa cập nhật'
      const [year, month, day] = dateString.split('-')
      return `${day}/${month}/${year}`
    }
  },
  mounted() {
    this.fetchUser()
  }
}
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap');

.account-view {
  max-width: 480px;
  min-width: 320px;
  margin: 40px auto;
  padding: 25px;
  background-color: #FFFFFF;
  border-radius: 12px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
  text-align: center;
  border: 1px solid #E5E7EB;
  transition: all 0.3s ease-in-out;
}

h1 {
  font-family: 'Inter', sans-serif;
  font-size: 24px;
  color: #1E3A8A;
  margin-bottom: 15px;
  font-weight: 600;
  text-transform: uppercase;
  letter-spacing: 1px;
}

p {
  font-family: 'Inter', sans-serif;
  font-size: 14px;
  color: #374151;
  padding: 10px;
  border-bottom: 1px solid #E5E7EB;
  text-align: left;
  margin: 0;
  font-weight: 500;
}

p:last-child {
  border-bottom: none;
}

p strong {
  color: #EF4444;
  font-weight: 600;
}

button {
  background-color: #1E3A8A;
  color: #FFFFFF;
  border: 2px solid #1E3A8A;
  padding: 10px 16px;
  border-radius: 8px;
  cursor: pointer;
  font-size: 14px;
  font-weight: 500;
  transition: all 0.2s ease-in-out;
}

button:hover {
  background-color: #1E40AF;
  border-color: #1E40AF;
  transform: scale(1.05);
}

button:active {
  background-color: #1E3A8A;
  transform: scale(0.98);
}

.avatar {
  width: 60px;
  height: 60px;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  font-family: 'Inter', sans-serif;
  font-size: 24px;
  font-weight: 600;
  color: #FFFFFF;
  text-transform: uppercase;
  margin: 0 auto 20px;
  background-color: #1E3A8A;
  box-shadow: 0 2px 6px rgba(0, 0, 0, 0.1);
  user-select: none;
  transition: transform 0.2s ease-in-out, box-shadow 0.2s ease-in-out;
}

.avatar:hover {
  transform: scale(1.1);
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.2);
}

@media (max-width: 600px) {
  .account-view {
    width: 90%;
    padding: 20px;
  }

  h1 {
    font-size: 20px;
  }

  p {
    font-size: 13px;
    padding: 8px;
  }

  button {
    font-size: 13px;
    padding: 8px 14px;
  }
}
</style>