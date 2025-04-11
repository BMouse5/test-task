<template>
  <button :class="colorClass">
    <slot></slot>
  </button>
</template>

<script setup>
defineProps({
    colorClass: {
        type: String,
        default: 'btn-green'
    }
})
</script>
<style lang="scss" scoped>
@use '/src/assets/css/root.scss' as *;
button {
    border: none;
    outline: none;
    border-radius: 5px;
    font-size: 24px;
    color: white;
    transition: all 0.2s ease-in-out;
    max-width: 273px;
    width: 100%;
    height: 52px;
    cursor: pointer;
    &:hover {
      opacity: 0.9;
    }
}

.btn-green {
  background-color: $btn-green-color;
}

.btn-gray {
  background-color: $btn-gray-color;
}
</style>