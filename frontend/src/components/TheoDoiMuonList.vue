<template>
  <div class="borrow-list">
    <h2>Theo dõi mượn sách</h2>

    <InputSearch v-model="search" />



    <table>
      <thead>
        <tr>
          <th>Người mượn</th>
          <th>Tên sách</th>
          <th>Số quyển</th>
          <th>Ngày mượn</th>
          <th>Ngày trả</th>
          <th>Trạng thái</th>
          <th>Thao tác</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="don in paginatedDonMuon" :key="don._id">
          <td>{{ loadHoTen(don.MADOCGIA) }}</td>
          <td>{{ loadSach(don.MASACH) }}</td>
          <td>{{ don.SOQUYEN }}</td>
          <td>{{ formatDate(don.NGAYMUON) }}</td>
          <td>{{ formatNgay(don.NGAYTRA) }}</td>
          <td>{{ don.TRANGTHAI }}</td>
          <td>
            <button
              style="background: #4CAF50; color: white"
              v-if="don.TRANGTHAI === 'Chờ duyệt'"
              @click="$emit('duyetMuon', don)"
            >
              Duyệt
            </button>
            <button
              style="background: #F44336; color: white"
              v-if="don.TRANGTHAI === 'Đang mượn'"
              @click="$emit('xacNhanTra', don)"
            >
              Trả
            </button>
            <button
              style="background: #FF9800; color: white"
              @click="$emit('xoaDonMuon', don)"
            >
              Xóa
            </button>
          </td>
        </tr>
      </tbody>
    </table>

    <div class="pagination mt-3">
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
    danhSachDonMuon: Array
  },
  data() {
    return {
      currentPage: 1,
      perPage: 10,
      search: ''
    }
  },
  computed: {
    filteredDonMuon() {
      const keyword = this.search.toLowerCase().trim()
      return this.danhSachDonMuon.filter(don => {
        const tenSach = don.MASACH?.TENSACH?.toLowerCase() || ''
        const hoTen = don.MADOCGIA
          ? `${don.MADOCGIA.HOLOT} ${don.MADOCGIA.TEN}`.toLowerCase()
          : ''
        const trangThai = don.TRANGTHAI?.toLowerCase() || ''
        const ngayMuon = this.formatDate(don.NGAYMUON).toLowerCase()
        const ngayTra = this.formatNgay(don.NGAYTRA).toLowerCase()

        return (
          tenSach.includes(keyword) ||
          ngayMuon.includes(keyword) ||
          ngayTra.includes(keyword) ||
          hoTen.includes(keyword) ||
          trangThai.includes(keyword)
        )
      })
    },
    totalPages() {
      return Math.ceil(this.filteredDonMuon.length / this.perPage)
    },
    paginatedDonMuon() {
      const start = (this.currentPage - 1) * this.perPage
      const end = start + this.perPage
      return this.filteredDonMuon.slice(start, end)
    }
  },
  methods: {
    loadHoTen(docGia) {
      if (!docGia || typeof docGia !== 'object') return 'Không tìm thấy'
      return `${docGia.HOLOT} ${docGia.TEN}`
    },
    loadSach(maSach) {
      if (!maSach || typeof maSach !== 'object') return 'Không tìm thấy'
      return `${maSach.TENSACH}`
    },
    formatDate(date) {
      return new Date(date).toLocaleDateString('vi-VN')
    },
    formatNgay(ngay) {
      if (!ngay || ngay === 'null') return 'Chưa trả'
      return new Date(ngay).toLocaleDateString('vi-VN')
    },
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
  font-weight: 600;
  color: #1E3A8A;
}

.borrow-list {
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

button {
  padding: 6px 12px;
  margin: 4px;
  font-size: 14px;
  border: none;
  cursor: pointer;
  border-radius: 6px;
  transition: opacity 0.2s, background-color 0.2s;
}

button:hover {
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
  transition: background-color 0.2s;
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
</style>