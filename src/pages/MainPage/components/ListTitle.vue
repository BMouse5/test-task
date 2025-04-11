<template>
  <div class="table-header">
    <div class="header-cell">№</div>
    <div class="header-cell">ФИО</div>
    <div class="header-cell">Компания</div>
    <div class="header-cell">Группа</div>
    <div class="header-cell">Присутствие</div>
  </div>
</template>
  
<script setup>

</script>
  
<style lang="scss" scoped>
@use '/src/assets/css/root.scss' as *;
.table-header {
  display: grid;
  grid-template-columns: 50px 2fr 2fr 1fr 1fr;
  gap: 15px;
  padding: 12px 15px;
  border-bottom: 2px solid $border-color;
  font-weight: bold;
  border-radius: 8px 8px 0 0;
  color: $second-text-color;

  .header-cell {
    padding: 8px;
    font-size: 20px;

    &:last-child {
        text-align: right;
    }
  }
}
</style>