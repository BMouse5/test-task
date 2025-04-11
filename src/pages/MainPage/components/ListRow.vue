<template>
  <div class="table-row" @click="openPopup">
    <div class="table-cell">{{ client.id }}</div>
    <div class="table-cell">{{ client.name }}</div>
    <div class="table-cell">{{ client.company }}</div>
    <div class="table-cell">{{ client.group }}</div>
    <div class="table-cell">
      <span :class="['presence-indicator', client.isPresent ? 'present' : 'absent']"></span>
    </div>
  </div>
  <!-- попап для редактирования информации о человеке -->
  <Popup ref="popup">
    <form @submit.prevent="saveClient" class="popup-form">
      <div class="input-group">
        <label>ФИО</label>
        <InputComp v-model="editableClient.name"></InputComp>
      </div>
      <div class="input-group">
        <label>Компания</label>
        <InputComp v-model="editableClient.company"></InputComp>
      </div>
      <div class="input-group">
        <label>Группа</label>
        <InputComp v-model="editableClient.group" type="select"></InputComp>
      </div>
      <div class="input-group">
        <label>Присутствовал</label>
        <InputComp v-model="editableClient.isPresent" type="checkbox"></InputComp>
      </div>
      <ButtonComp>Сохранить</ButtonComp>
    </form>
  </Popup>
</template>

<script setup>
import { ref, watch } from 'vue'
import Popup from '../../../components/Popup.vue'
import InputComp from '../../../components/ui/InputComp.vue'
import ButtonComp from '../../../components/ui/ButtonComp.vue'

const props = defineProps({
  client: {
    type: Object,
    required: true
  }
})

const emit = defineEmits(['update-client'])
const popup = ref(null)

const editableClient = ref({...props.client}) //создаем локальную копию списка клиентов

//Обновляем локальную копию при изменении пропса
watch(() => props.client, (newVal) => {
  editableClient.value = {...newVal}
}, { deep: true })

const openPopup = () => {
  popup.value.openPopup()
}
//отправляем эми в ClientList
const saveClient = () => {
  if (!editableClient.value.name) return
  
  emit('update-client', {
    ...editableClient.value,
    id: props.client.id
  })
  
  popup.value.closePopup()
}
</script>

<style lang="scss" scoped>
@use '/src/assets/css/root.scss' as *;

.table-row {
  display: grid;
  grid-template-columns: 50px 2fr 2fr 1fr 1fr;
  gap: 15px;
  padding: 12px 15px;
  align-items: center;
  cursor: pointer;
  

  &:hover {
    background-color: $table-row-color;
  }
}

.table-cell {
  padding: 8px;
  font-size: 30px;
}

.table-cell:last-child {
  display: flex;
  justify-content: right;
  margin-right: 24px;
}

.presence-indicator {
  width: 16px;
  height: 16px;
  border-radius: 50%;
  display: inline-block;

  &.present {
    background-color: $primary-green;
    max-width: 40px;
    width: 100%;
    height: 40px;
  }

  &.absent {
    background-color: $primary-red;
    max-width: 40px;
    width: 100%;
    height: 40px;
  }
}


.popup-form {
  padding: 30px 100px;
  display: flex;
  flex-direction: column;
  gap: 16px;
  min-width: 800px;
  
  .input-group {
    display: flex;
    align-items: center;
    gap: 60px;
    
    label {
      font-size: 18px;
      color: $second-text-color;
      min-width: 120px;
    }
    
    &:not(:last-child) {
      padding-bottom: 20px;
    }
    

    &:last-child {
      margin-top: 10px;
      justify-content: flex-start;
      gap: 15px;
      
      label {
        order: 2;
      }
    }
  }
  

}
</style>