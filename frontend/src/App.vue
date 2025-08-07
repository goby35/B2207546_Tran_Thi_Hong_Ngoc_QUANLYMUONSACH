<script setup>
import HeaderComponent from './components/HeaderView.vue'
import FooterComponent from './components/FooterView.vue'
</script>

<template>
  <div class="wrapper">
    <HeaderComponent />

      <main class="content">
      <div class="container mt-4">
        <router-view />
      </div>
    </main>

    <FooterComponent />
  </div>
</template>

<style>
@import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap');

body {
  background-color: #F3F4F6;
  margin: 0;
  font-family: 'Inter', sans-serif;
}

.wrapper {
  display: flex;
  flex-direction: column;
  min-height: 100vh;
}

.content {
  flex: 1;
  padding-bottom: 20px; /* Đảm bảo khoảng cách với footer */
}

.container {
  max-width: 1280px;
  margin-left: auto;
  margin-right: auto;
  padding-left: 15px;
  padding-right: 15px;
}
</style>