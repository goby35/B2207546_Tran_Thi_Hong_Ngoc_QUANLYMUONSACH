<template>
  <div class="input-group mb-3">
    <input type="text" class="form-control" placeholder="Tìm kiếm sách" :value="modelValue"
      @input="updateModelValue" @keyup.enter="submit" />
    <button class="btn btn-outline-secondary" @click="submit">Tìm</button>
  </div>
</template>

<script setup>
defineProps({
  modelValue: { type: String, default: "" },
});
const emit = defineEmits(["update:modelValue", "submit"]);

const updateModelValue = (e) => {
  emit("update:modelValue", e.target.value);
};

const submit = () => {
  emit("submit");
};
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap');

.input-group {
  max-width: 400px;
  margin: 0 auto;
  display: flex;
  align-items: center;
}

.form-control {
  border: 2px solid #D1D5DB;
  border-right: none;
  border-radius: 8px 0 0 8px;
  padding: 12px 16px;
  font-family: 'Inter', sans-serif;
  font-size: 16px;
  color: #374151;
  background-color: #F3F4F6;
  flex: 1;
  height: 48px;
}

.form-control:focus {
  box-shadow: none;
  border-color: #9CA3AF;
  background-color: #FFFFFF;
}

.btn-outline-secondary {
  border: 2px solid #1E3A8A;
  border-left: none;
  border-radius: 0 8px 8px 0;
  background-color: #1E3A8A;
  color: #FFFFFF;
  font-family: 'Inter', sans-serif;
  font-weight: 500;
  padding: 12px 16px;
  height: 48px;
  min-width: 80px;
  transition: background-color 0.2s, border-color 0.2s;
}

.btn-outline-secondary:hover {
  background-color: #1E40AF;
  border-color: #1E40AF;
}
</style>