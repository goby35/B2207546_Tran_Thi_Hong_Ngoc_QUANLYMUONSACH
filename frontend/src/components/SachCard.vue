<template>
  <div class="sach-card">
    <div class="sach-content">
      <div class="sach-image-container">
        <img :src="`http://localhost:3000${sach.HINHANH}`" alt="Hình ảnh sách" class="sach-image">
      </div>
      <div class="sach-info">
        <h2 class="sach-title">{{ sach.TENSACH }}</h2>
        <p class="sach-detail"><strong>Tác giả:</strong> {{ sach.NGUONGOC_TACGIA || "Không rõ" }}</p>
        <p class="sach-detail"><strong>Năm Xuất Bản:</strong> {{ sach.NAMXUATBAN || "Không rõ" }}</p>
        <p class="sach-detail"><strong>Nhà Xuất Bản:</strong> {{ getNXBName(sach.MANXB) || "Không rõ" }}</p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    sach: Object,
    nxbs: Object
  },
  methods: {
    getNXBName(book) {
      if (!book || !book.MANXB) return "Chưa có NXB";
      const manxb = typeof book.MANXB === "object" ? book.MANXB.MANXB : book.MANXB;
      const nxb = this.nxbs.find(n => n.MANXB === manxb || n._id === manxb || String(n._id) === String(manxb));
      return nxb ? nxb.TENNXB : "Không tìm thấy";
    }
  }
};
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap');

.sach-card {
  width: 340px;
  background: #FFFFFF;
  border-radius: 12px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.06);
  overflow: hidden;
  transition: transform 0.3s ease, box-shadow 0.3s ease;
  border-left: 4px solid #1E3A8A;
  display: flex;
  align-items: center;
}

.sach-card:hover {
  transform: translateY(-5px);
  box-shadow: 0 6px 18px rgba(0, 0, 0, 0.1);
}

.sach-content {
  display: flex;
  align-items: center;
  padding: 15px;
  width: 100%;
}

.sach-image-container {
  flex: 1;
  position: relative;
  margin-right: 15px;
}

.sach-image {
  width: 100%;
  max-width: 140px;
  height: auto;
  border-radius: 8px;
  transition: transform 0.3s ease;
}

.sach-card:hover .sach-image {
  transform: scale(1.05);
}

.sach-info {
  flex: 2;
  text-align: left;
  padding: 5px 0;
}

.sach-title {
  font-size: 20px;
  font-weight: 700;
  color: #1E3A8A;
  margin-bottom: 8px;
  font-family: 'Inter', sans-serif;
}

.sach-detail {
  font-size: 14px;
  color: #4B5563;
  margin: 4px 0;
  font-family: 'Inter', sans-serif;
}
</style>