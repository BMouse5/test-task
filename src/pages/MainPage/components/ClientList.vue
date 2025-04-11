<template>
  <div class="clients-table">
    <ListTitle />
    <ListRow 
      v-for="client in clients" 
      :key="client.id"
      :client="client"
      @update-client="$emit('update-client', $event)"
    />
    <!--отправляем эмит в MainPage-->
  </div>
</template>

<script setup>
import { ref } from 'vue'
import ListTitle from './ListTitle.vue'
import ListRow from './ListRow.vue'

defineProps({
  clients: {
    type: Array,
    required: true
  }
})

defineEmits(['update-client'])
</script>

<style lang="scss" scoped>
.clients-table {
  width: 100%;
  border-collapse: collapse;
  margin-top: 20px;
}
</style>