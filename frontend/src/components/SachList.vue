<template>

  <div>
    <table class="table table-bordered">
      <thead class="thead-light">
        <tr>
          <th>Mã Sách</th>
          <th>Tên Sách</th>
          <th>Tác giả</th>
          <th>Nhà Xuất Bản</th>
          <th>Số Lượng</th>
          <th>Đơn Giá</th>
          <th>Thao Tác</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="book in paginatedBooks" :key="book._id">
          <td>{{ book.MASACH || 'Không có mã' }}</td>
          <td>{{ book.TENSACH || 'Không có tên' }}</td>
          <td>{{ book.NGUONGOC_TACGIA || 'Không có tên' }}</td>
          <td>{{ getNXBName(book.MANXB) }}</td>
          <td>
            {{ book.SOQUYEN !== undefined ? book.SOQUYEN : 'Không có dữ liệu' }}
          </td>
          <td>{{ formatCurrency(book.DONGIA) }}</td>
          <td>
            <div class="button-group">
              <button
                class="btn btn-warning btn-sm mx-1"
                @click="$emit('edit', book)"
              >
                Sửa
              </button>
              <button
                class="btn btn-danger btn-sm"
                @click="$emit('delete', book._id)"
              >
                Xóa
              </button>
            </div>
          </td>
        </tr>
      </tbody>
    </table>

    <div class="pagination">
      <button @click="prevPage" :disabled="currentPage === 1">
        <i class="bi bi-chevron-left"></i>
      </button>
      <span>Trang {{ currentPage }} / {{ totalPages }}</span>
      <button @click="nextPage" :disabled="currentPage === totalPages">
        <i class="bi bi-chevron-right"></i>
      </button>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    books: { type: Array, required: true },
    nxbs: { type: Array, required: true }
  },
  data() {
    return {
      currentPage: 1,
      perPage: 10
    }
  },
  computed: {
    totalPages() {
      return Math.ceil(this.books.length / this.perPage)
    },
    paginatedBooks() {
      const start = (this.currentPage - 1) * this.perPage
      return this.books.slice(start, start + this.perPage)
    }
  },
  emits: ['edit', 'delete'],
  methods: {
    prevPage() {
      if (this.currentPage > 1) this.currentPage--
    },
    nextPage() {
      if (this.currentPage < this.totalPages) this.currentPage++
    },
    getNXBName(book) {
      if (!book || !book.MANXB) return 'Chưa có NXB'

      const manxb =
        typeof book.MANXB === 'object' ? book.MANXB.MANXB : book.MANXB

      const nxb = this.nxbs.find(
        n =>
          n.MANXB === manxb ||
          n._id === manxb ||
          String(n._id) === String(manxb)
      )
      return nxb ? nxb.TENNXB : 'Không tìm thấy'
    },
    formatCurrency(value) {
      if (value == null || isNaN(value)) return 'Không có giá'
      return new Intl.NumberFormat('vi-VN', {
        style: 'currency',
        currency: 'VND'
      }).format(value)
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

.table {
  margin-top: 15px;
  text-align: center;
}

th,
td {
  vertical-align: middle;
  font-family: 'Inter', sans-serif;
  font-size: 14px;
  color: #374151;
}

th {
  background-color: #F3F4F6;
  font-weight: 500;
}

.button-group {
  display: flex;
  justify-content: space-between;
  margin-top: 5px;
}

.pagination {
  display: flex;
  justify-content: center;
  margin-top: 20px;
}

.pagination button {
  padding: 10px 15px;
  margin: 5px;
  border: none;
  background: #1E3A8A;
  color: #FFFFFF;
  cursor: pointer;
  border-radius: 6px;
  transition: background-color 0.2s ease-in-out;
  font-family: 'Inter', sans-serif;
  font-weight: 500;
}

.pagination button:disabled {
  background: #D1D5DB;
  cursor: not-allowed;
}

.pagination span {
  margin: 15px;
  font-family: 'Inter', sans-serif;
  font-size: 14px;
  color: #374151;
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