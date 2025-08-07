<template>
  <div class="container text-center mt-5">
    <div class="hero-banner">
      <img
        src="../assets/55c6e6b947eeda67b1bfa761c1013663.jpg"
        class="hero-image"
        alt="Hero Banner"
      />
      <div class="hero-caption">
        <h2>HỆ THỐNG QUẢN LÝ MƯỢN SÁCH</h2>
      </div>
    </div>

    <InputSearch v-model="search" class="mt-4" />
    <h2 v-if="userRole === 'quanly'">
      Xin chào, bạn đã đăng nhập thành công với vai trò quản lý thư viện!
    </h2>
    <h2 v-else-if="userRole === 'nhanvien'">
      Xin chào, bạn đã đăng nhập thành công với vai trò nhân viên thư viện!
    </h2>
    <h2 v-else>
      Thư viện là nơi những ý tưởng lớn bắt đầu từ những trang giấy nhỏ.
    </h2>

    <div class="sach-list">
      <SachCard
        v-for="sach in filteredBooks"
        :key="sach.MASACH"
        :sach="sach"
        :nxbs="nxbs"
      />
    </div>
  </div>
</template>

<script>
import SachCard from '@/components/SachCard.vue';
import InputSearch from '@/components/InputSearch.vue';
import { fetchBooks } from '@/services/sachService';
import { fetchNXB } from '@/services/nxbService';

export default {
  components: {
    SachCard,
    InputSearch
  },
  data() {
    return {
      sachList: [],
      search: '',
      nxbs: []
    };
  },
  mounted() {
    this.loadBooks();
    this.loadNXBs();
  },
  computed: {
    userRole() {
      return this.$store.state.user.role;
    },
    filteredBooks() {
      // Lấy danh sách đã xáo trộn và giới hạn 10 quyển
      const shuffled = [...this.sachList].sort(() => Math.random() - 0.5);
      const limitedBooks = shuffled.slice(0, 10);

      // Lọc theo từ khóa tìm kiếm
      return limitedBooks.filter(book => {
        const keyword = this.search.toLowerCase().trim();
        const manxbValue = book.MANXB?._id || book.MANXB?.MANXB || '';

        const nxb = this.nxbs.find(n => String(n._id) === String(manxbValue));
        const tenNXB = nxb ? nxb.TENNXB.toLowerCase() : '';
        return (
          book.TENSACH?.toLowerCase().includes(keyword) ||
          book.MASACH?.toLowerCase().includes(keyword) ||
          book.NGUONGOC_TACGIA?.toLowerCase().includes(keyword) ||
          (String(book.NAMXUATBAN) || '').toLowerCase().includes(keyword) ||
          tenNXB.includes(keyword)
        );
      });
    }
  },
  methods: {
    async loadBooks() {
      try {
        this.sachList = await fetchBooks();
      } catch (error) {
        console.error('Lỗi khi tải sách:', error);
      }
    },
    async loadNXBs() {
      try {
        this.nxbs = await fetchNXB();
      } catch (error) {
        console.error('Lỗi khi tải nhà xuất bản:', error);
      }
    }
  }
};
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap');

h2 {
  font-family: 'Inter', sans-serif;
  font-size: 20px;
  font-weight: 600;
  color: #1E3A8A;
  margin-bottom: 20px;
}

.card {
  max-width: 500px;
  margin: auto;
  border-radius: 8px;
}

.sach-list {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 20px;
  margin-top: 20px;
  justify-content: center;
}

.hero-banner {
  position: relative;
  margin-bottom: 20px;
}

.hero-image {
  width: 100%;
  height: 400px;
  object-fit: cover;
  filter: brightness(0.7);
  border-radius: 8px;
}

.hero-caption {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  text-align: center;
  color: #FFFFFF;
  text-shadow: 2px 2px 10px rgba(0, 0, 0, 0.8);
}

.hero-caption h2 {
  font-family: 'Inter', sans-serif;
  font-size: 32px;
  font-weight: 600;
  color: #FFFFFF;
}

.hero-caption p {
  font-family: 'Inter', sans-serif;
  font-size: 16px;
  color: #FFFFFF;
}
</style>