<template>
  <div class="nhanvien-list">
    <h2>Danh sách nhân viên</h2>

    <InputSearch v-model="search" />


    <table>
      <thead>
        <tr>
          <th>Họ tên</th>
          <th>Số điện thoại</th>
          <th>Chức vụ</th>
          <th>Thao tác</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="nhanvien in paginatedNhanViens" :key="nhanvien._id">
          <td>{{ nhanvien.HOTENNV }}</td>
          <td>{{ nhanvien.SODIENTHOAI }}</td>
          <td>
            {{ nhanvien.CHUCVU === 'QuanLyThuVien' ? 'Quản lý' : 'Nhân viên' }}
          </td>
          <td>
            <button
              class="delete-btn"
              @click="$emit('deleteNhanVien', nhanvien._id)"
            >
              Xóa
            </button>
          </td>
        </tr>
      </tbody>
    </table>

    <div class="pagination">
      <button @click="prevPage" :disabled="currentPage === 1">
        <i class="bi bi-chevron-left"></i>
        Trước
      </button>
      <span>Trang {{ currentPage }} / {{ totalPages }}</span>
      <button @click="nextPage" :disabled="currentPage === totalPages">
        Sau
        <i class="bi bi-chevron-right"></i>
      </button>
    </div>
  </div>
</template>

<script>
import InputSearch from '@/components/InputSearch.vue'

export default {
  components: { InputSearch },
  props: {
    nhanViens: { type: Array, required: true }
  },
  data() {
    return {
      currentPage: 1,
      perPage: 5,
      search: ''
    }
  },
  computed: {
    filteredNhanViens() {
      const keyword = this.search.toLowerCase().trim()
      return this.nhanViens.filter(nhanvien => {
        const hoTen = nhanvien.HOTENNV ? nhanvien.HOTENNV.toLowerCase() : ''
        const soDienThoai = nhanvien.SODIENTHOAI
          ? nhanvien.SODIENTHOAI.toLowerCase()
          : ''
        const chucVu =
          nhanvien.CHUCVU === 'QuanLyThuVien' ? 'quản lý' : 'nhân viên'

        return (
          hoTen.includes(keyword) ||
          soDienThoai.includes(keyword) ||
          chucVu.includes(keyword)
        )
      })
    },
    totalPages() {
      return Math.ceil(this.nhanViens.length / this.perPage)
    },
    paginatedNhanViens() {
      const start = (this.currentPage - 1) * this.perPage
      const end = start + this.perPage
      return this.filteredNhanViens.slice(start, end)
    }
  },
  methods: {
    prevPage() {
      if (this.currentPage > 1) {
        this.currentPage--
      }
    },
    nextPage() {
      if (this.currentPage < this.totalPages) {
        this.currentPage++
      }
    }
  },
  watch: {
    perPage() {
      this.currentPage = 1
    }
  }
}
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap');

h2 {
  text-align: center;
  margin-bottom: 20px;
  font-family: 'Inter', sans-serif;
  font-size: 24px;
  color: #1E3A8A;
  font-weight: 600;
}

.nhanvien-list {
  padding: 20px;
}

table {
  width: 100%;
  border-collapse: collapse;
  background-color: #FFFFFF;
  border-radius: 8px;
  overflow: hidden;
}

th,
td {
  border: 1px solid #E5E7EB;
  padding: 12px;
  text-align: left;
  font-family: 'Inter', sans-serif;
  font-size: 14px;
  color: #374151;
}

th {
  background: #F3F4F6;
  font-weight: 500;
}

th:last-child,
td:last-child {
  width: 150px;
  text-align: center;
  padding: 8px;
}

.delete-btn {
  background: #DC3545;
  color: #FFFFFF;
  border: none;
  padding: 6px 12px;
  cursor: pointer;
  border-radius: 6px;
  transition: opacity 0.2s ease-in-out;
}

.delete-btn:hover {
  opacity: 0.9;
}

.pagination {
  display: flex;
  justify-content: center;
  align-items: center;
  margin-top: 20px;
  gap: 12px;
}

.pagination button {
  padding: 8px 12px;
  border: none;
  background-color: #1E3A8A;
  color: #FFFFFF;
  border-radius: 6px;
  cursor: pointer;
  transition: background-color 0.2s ease-in-out;
  display: flex;
  align-items: center;
  gap: 6px;
  font-family: 'Inter', sans-serif;
  font-weight: 500;
}

.pagination button:disabled {
  background-color: #D1D5DB;
  cursor: not-allowed;
}

.page-size {
  display: flex;
  align-items: center;
  justify-content: flex-start;
  margin-bottom: 15px;
  font-size: 16px;
}

.page-size label {
  margin-right: 12px;
  font-weight: 500;
  font-family: 'Inter', sans-serif;
  color: #1E3A8A;
}

.page-size select {
  padding: 6px 12px;
  border: 1px solid #D1D5DB;
  border-radius: 6px;
  font-size: 14px;
  background: #FFFFFF;
  cursor: pointer;
  font-family: 'Inter', sans-serif;
}

.page-size span {
  margin-left: 12px;
  font-size: 14px;
  color: #555;
  font-family: 'Inter', sans-serif;
}

button:hover {
  opacity: 0.8;
}
</style>