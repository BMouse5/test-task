<template>
  <div class="popup" v-if="isVisible">
    <div class="popup-wrapp">
      <slot></slot>
        <button class="close-btn" @click="closePopup">×</button>
    </div>
  </div>
</template>
  
<script setup>
import { ref } from 'vue'
  
const isVisible = ref(false)
  
const openPopup = () => {
  isVisible.value = true
  document.body.style.overflow = 'hidden'
}
  
const closePopup = () => {
  isVisible.value = false
  document.body.style.overflow = ''
}
  
defineExpose({
  openPopup,
  closePopup
})
</script>
  
<style lang="scss" scoped>
@use '../assets/css/root.scss' as *;
.popup {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 1000;

  .popup-wrapp {
    position: relative;
    background: $primary-white;
    padding: 24px;
    border-radius: 30px;
    min-width: 800px;
    max-width: 90%;
    width: auto;
    display: flex;
    justify-content: center;
    max-height: 80%;
    overflow: auto;

    .close-btn {
      position: absolute;
      top: 10px;
      right: 10px;
      background: none;
      border: none;
      font-size: 20px;
      cursor: pointer;
      padding: 4px;
      background-color: $close-btn;
      border-radius: 50px;
      width: 40px;
      height: 40px;
      color: $primary-white;
    }
  }
}
</style>