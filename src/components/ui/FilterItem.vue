<template>
  <a href="">
    <slot></slot>
  </a>
</template>

<script setup>

</script>

<style lang="scss" scoped>
@use '/src/assets/css/root.scss' as *;
a {
    display: flex;
    gap: 65px;
}
</style>