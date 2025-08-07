<template>
  <div class="borrow-container">
    <h2>Đăng Ký Mượn Sách</h2>

    <button class="btn btn-add" @click="showBorrowForm = true">
      Thêm Phiếu Mượn
    </button>

    <div v-if="showBorrowForm" class="borrow-form">
      <h3>Phiếu Mượn</h3>
      
      <label for="book">Chọn sách:</label>
      <select v-model="selectedBook" @change="updateAvailableQuantity">
        <option v-for="book in books" :key="book._id" :value="book">
          {{ book.TENSACH }} (Còn: {{ book.SOQUYEN }})
        </option>
      </select>

      <label for="quantity">Số lượng mượn:</label>
      <input type="number" v-model.number="quantity" :max="selectedBook ? selectedBook.SOQUYEN : 1" min="1" />

      <button class="btn btn-success" @click="addToBorrowList">
        Thêm vào danh sách
      </button>

      <button class="btn btn-secondary" @click="showBorrowForm = false">
        Hủy
      </button>

      <div v-if="borrowList.length > 0">
        <h3>Danh Sách Mượn</h3>
        <ul>
          <li v-for="(item, index) in borrowList" :key="index">
            {{ item.book.TENSACH }} - {{ item.quantity }} cuốn
            <button class="btn btn-danger" @click="removeFromBorrowList(index)">Xóa</button>
          </li>
        </ul>

        <button class="btn btn-add" @click="registerBorrow">Đăng Ký Mượn</button>
        <button @click="cancelBorrow" class="btn-cancel">Hủy Phiếu Mượn</button>
      </div>
    </div>
  </div>
</template>

<script>
import { getBooks, borrowBooks } from "@/services/muonsachService";
import { useStore } from 'vuex';

export default {
  data() {
    const store = useStore(); 
    return {
      store, 
      showBorrowForm: false,
      books: [],
      selectedBook: null,
      quantity: 1,
      borrowList: [],
      ngayMuon: "",
    };
  },

  computed: {
    docGiaId() {
      return this.store.state.user._id; 
    },
  },

  methods: {
    async loadBooks() {
      this.books = await getBooks();
    },

    updateAvailableQuantity() {
      this.quantity = 1;
    },

    openBorrowForm() {
      const today = new Date();
      this.ngayMuon = today.toISOString().split("T")[0];
    },

    addToBorrowList() {
      if (!this.selectedBook || !this.selectedBook.TENSACH || this.quantity < 1) {
        alert("Vui lòng chọn sách hợp lệ và nhập số lượng!");
        return;
      }

      const existing = this.borrowList.find(item => item.book._id === this.selectedBook._id);
      if (existing) {
        existing.quantity += this.quantity;
      } else {
        this.borrowList.push({ book: this.selectedBook, quantity: this.quantity });
      }
      console.log("borrowList:", JSON.stringify(this.borrowList, null, 2));
    },

    removeFromBorrowList(index) {
      this.borrowList.splice(index, 1);
    },

    async registerBorrow() {
      this.openBorrowForm(); 

      if (this.borrowList.length === 0) {
        alert("Danh sách mượn trống!");
        return;
      }

      try {
        await borrowBooks(this.docGiaId, this.borrowList, this.ngayMuon);
        alert("Đăng ký mượn thành công! Vui lòng chờ duyệt.");
        this.borrowList = [];
        this.selectedBook = null;
        this.quantity = 1;
        this.showBorrowForm = false;
      } catch (error) {
        alert(error.response?.data?.message || "Lỗi không xác định!");
        console.error(error);
      }
    },

    cancelBorrow() {
      this.borrowList = [];
      this.selectedBook = null;
      this.quantity = 1;
    },
  },

  mounted() {
    this.loadBooks(); 
  },
};
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap');

.borrow-container {
  max-width: 800px;
  margin: 40px auto;
  padding: 20px;
  text-align: center;
  background: #F3F4F6;
  border-radius: 12px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
}

h2 {
  font-family: 'Inter', sans-serif;
  font-size: 24px;
  color: #1E3A8A;
  margin-bottom: 20px;
  font-weight: 600;
}

button {
  margin: 10px;
  padding: 10px 15px;
  border: none;
  border-radius: 8px;
  font-size: 14px;
  cursor: pointer;
  transition: all 0.2s ease-in-out;
  font-family: 'Inter', sans-serif;
  font-weight: 500;
}

.btn-add {
  background-color: #1E3A8A;
  color: #FFFFFF;
}

.btn-add:hover {
  background-color: #1E40AF;
}

.btn-success {
  background-color: #28A745;
  color: #FFFFFF;
}

.btn-success:hover {
  background-color: #218838;
}

.btn-danger {
  background-color: #DC3545;
  color: #FFFFFF;
}

.btn-danger:hover {
  background-color: #C82333;
}

.btn-secondary {
  background-color: #6C757D;
  color: #FFFFFF;
}

.btn-secondary:hover {
  background-color: #5A6268;
}

.btn-cancel {
  background-color: #6C757D;
  color: #FFFFFF;
}

.btn-cancel:hover {
  background-color: #5A6268;
}

.borrow-form {
  background: #FFFFFF;
  padding: 15px;
  margin-top: 20px;
  border-radius: 8px;
  box-shadow: 0 2px 6px rgba(0, 0, 0, 0.1);
}

label {
  font-weight: 500;
  display: block;
  margin-top: 15px;
  font-family: 'Inter', sans-serif;
  color: #374151;
}

select, input {
  width: 100%;
  padding: 10px;
  margin-top: 5px;
  border: 1px solid #D1D5DB;
  border-radius: 6px;
  font-family: 'Inter', sans-serif;
  font-size: 14px;
}

ul {
  list-style: none;
  padding: 0;
}

li {
  background: #E5E7EB;
  padding: 12px;
  margin: 8px 0;
  border-radius: 6px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  font-family: 'Inter', sans-serif;
  font-size: 14px;
  color: #374151;
}

li button {
  font-size: 14px;
}
</style>