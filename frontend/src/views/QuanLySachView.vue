<template>
  <div>
    <h2>Quản Lý Sách</h2>

    <InputSearch v-model="search" />

    <button @click="openBookForm(null)" class="btn btn-add">Thêm Sách</button>

    <button @click="showNxbForm = true" class="btn btn-success ml-2">
      Thêm Nhà Xuất Bản
    </button>

    <SachForm
      v-if="showBookForm"
      :book="selectedBook"
      :nxbList="nxbs"
      @submit="saveBook"
      @cancel="closeBookForm"
      :key="selectedBook?._id || 'new'"
    />

    <NxbForm
      v-if="showNxbForm"
      @submit="saveNxb"
      @cancel="showNxbForm = false"
    />

    <SachList
      :books="filteredBooks"
      :nxbs="nxbs"
      @edit="openBookForm"
      @delete="deleteBook"
    />
  </div>
</template>

<script>
import InputSearch from '@/components/InputSearch.vue'
import SachForm from '@/components/SachForm.vue'
import NxbForm from '@/components/NxbForm.vue'
import SachList from '@/components/SachList.vue'
import {
  fetchBooks,
  createBook,
  updateBook,
  deleteBook
} from '@/services/sachService'
import { fetchNXB, createNXB } from '@/services/nxbService'

export default {
  components: { SachForm, NxbForm, SachList, InputSearch },
  data() {
    return {
      search: '',
      books: [],
      nxbs: [],
      showBookForm: false,
      showNxbForm: false,
      selectedBook: null,
      formKey: 0,
      saving: false
    }
  },
  computed: {
    filteredBooks() {
      return this.books.filter(book => {
        const tenSach = book.TENSACH ? book.TENSACH.toLowerCase() : ''
        const maSach = book.MASACH ? book.MASACH.toLowerCase() : ''
        const tacGia = book.NGUONGOC_TACGIA
          ? book.NGUONGOC_TACGIA.trim().toLowerCase()
          : ''
        const keyword = this.search.toLowerCase().trim()

        const manxbValue = book.MANXB?._id || book.MANXB?.MANXB || ''
        const nxb = this.nxbs.find(n => String(n._id) === String(manxbValue))
        const tenNXB = nxb ? nxb.TENNXB.toLowerCase() : ''

        return (
          tenSach.includes(keyword) ||
          maSach.includes(keyword) ||
          tenNXB.includes(keyword) ||
          tacGia.includes(keyword)
        )
      })
    }
  },
  methods: {
    async loadBooks() {
      try {
        this.books = await fetchBooks()
      } catch (error) {
        console.error('Lỗi khi tải sách:', error)
      }
    },
    async loadNXBs() {
      try {
        this.nxbs = await fetchNXB()
      } catch (error) {
        console.error('Lỗi khi tải nhà xuất bản:', error)
      }
    },
    getNXBName(manxb) {
      const nxb = this.nxbs.find(
        n => String(n._id) === String(manxb?.MANXB || manxb)
      )
      return nxb ? nxb.TENNXB : 'Không tìm thấy'
    },
    async openBookForm(book) {
      await this.loadNXBs()
      this.selectedBook = book ? { ...book } : null
      this.showBookForm = true
    },
    closeBookForm() {
      this.selectedBook = null
      this.showBookForm = false
    },
    async saveBook(book) {
      try {
        if (this.saving) return
        this.saving = true
        if (book._id) {
          await updateBook(book)
          alert('Cập nhật sách thành công!')
        } else {
          await createBook(book)
          alert('Thêm sách mới thành công!')
        }
        this.closeBookForm()
        await this.loadBooks()
      } catch (error) {
        console.error(
          'Lỗi chi tiết:',
          error.response ? error.response.data : error.message
        )
        alert(
          'Có lỗi xảy ra khi lưu sách: ' +
            (error.response?.data?.message || error.message)
        )
      } finally {
          this.saving = false;
      }
    },
    async saveNxb(nxb) {
      try {
        const newNXB = await createNXB(nxb)
        this.nxbs.push(newNXB)
        alert('Thêm nhà xuất bản mới thành công!')
        this.showNxbForm = false
      } catch (error) {
        console.error('Lỗi khi thêm nhà xuất bản:', error)
      }
    },
    async deleteBook(id) {
      try {
        await deleteBook(id)
        alert('Xóa sách thành công!')
        await this.loadBooks()
        this.loadBooks()
      } catch (error) {
        console.error('Lỗi khi xóa sách:', error)
      }
    }
  },
  created() {
    this.loadBooks()
    this.loadNXBs()
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

button {
  margin-bottom: 10px;
  font-family: 'Inter', sans-serif;
  font-size: 14px;
  font-weight: 500;
  transition: all 0.2s ease-in-out;
}

.btn-add {
  background-color: #1E3A8A;
  border: none;
  padding: 10px 15px;
  border-radius: 6px;
  cursor: pointer;
  color: #FFFFFF;
}

.btn-add:hover {
  background-color: #1E40AF;
}

.btn-success {
  background-color: #28A745;
  border: none;
  padding: 10px 15px;
  border-radius: 6px;
  cursor: pointer;
  color: #FFFFFF;
}

.btn-success:hover {
  background-color: #218838;
}

.ml-2 {
  margin-left: 10px;
}
</style>