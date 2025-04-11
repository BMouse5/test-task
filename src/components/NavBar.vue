<template>
  <div class="navbar">
    <div class="navbar-wrapp">
        <div class="navbar-inputs">
            <img src="/src/assets/img/logo.svg" alt="">
            <InputComp 
                placeholderText="Поиск по имени"
                v-model="searchQuery"
                @update:modelValue="nameSearch"
            >
        </InputComp>
            <ButtonComp @click="openPopup">Добавить</ButtonComp>
        </div>
        <div class="navbar-clients">
            <h4>Посетители</h4>
            <span>
              <span class="present-count">{{ presentCount }}</span> / 
              <span class="absent-count">{{ absentCount }}</span>
            </span>
        </div>
    </div>
  </div>
  <!-- попап для добавления человека в список -->
  <Popup ref="popup">
    <form @submit.prevent="submitForm" class="popup-form">
      <div class="input-group">
        <label>ФИО</label>
        <InputComp v-model="newClient.name"></InputComp>
      </div>
      <div class="input-group">
        <label>Компания</label>
        <InputComp v-model="newClient.company"></InputComp>
      </div>
      <div class="input-group">
        <label>Группа</label>
        <InputComp type="select" v-model="newClient.group"></InputComp>
      </div>
      <div class="input-group">
        <label>Присутствовал</label>
        <InputComp type="checkbox" v-model="newClient.isPresent"></InputComp>
      </div>
      <ButtonComp>Добавить</ButtonComp>
    </form>
  </Popup>
</template>

<script setup>
import { ref, computed } from 'vue'
import ButtonComp from './ui/ButtonComp.vue'
import InputComp from './ui/InputComp.vue'
import Popup from './Popup.vue'

const popup = ref(null)
const searchQuery = ref('')

const props = defineProps({
    clients: {
        type: Array,
        required: true
    }
})

const openPopup = () => {
  popup.value.openPopup()
}

const nameSearch = () => {
  emit('search', searchQuery.value)
}

const emit = defineEmits(["add-client"])

const newClient = ref({
    name: '',
    company: '',
    group: '',
    isPresent: false
})
//отправляем эмит в главный компонент для добавления клиента
const submitForm = () => {
    if (!newClient.value.name) return

    emit('add-client', {
        ...newClient.value,
        group: newClient.value.group
    })
    popup.value.closePopup()
    newClient.value = {
        name: '',
        company: '',
        group: '',
        isPresent: false
    }
}
//отображение в nav кол-во человек по фильтру "присутствовал", "отсутствовал"
const presentCount = computed(() => props.clients.filter(client => client.isPresent).length)
const absentCount = computed(() => props.clients.filter(client => !client.isPresent).length)
</script>

<style lang="scss" scoped>
@use '/src/assets/css/root.scss' as *; 
.navbar-wrapp {
    display: flex;
    justify-content: space-between;
    align-items: center;
    .navbar-inputs {
        display: flex;
        width: 100%;
        gap: 30px;
        align-items: center;
        flex-wrap: wrap;
    }

    .navbar-clients {
        display: flex;
        flex-direction: column;
        gap: 5px;
        font-weight: 700;
        span {
            text-align: right;
            color: $second-text-color;
            font-size: 30px;
            .present-count {
                color: $primary-green; 
            }
            
            .absent-count {
                color: $primary-red; 
            }
        }
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