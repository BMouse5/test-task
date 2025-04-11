<template>
  <div class="container">
    <NavBar @search="searchClient" :clients="clients" @add-client="addClient"></NavBar>
    <main class="main-content">
      <ClientList :clients="filteredClients" @update-client="updateClient"></ClientList>
    </main>
    <FooterComp :activeFilter="activeFilter" @update:activeFilter="setActiveFilter"></FooterComp>
  </div>
</template>

<script setup>
import { ref, watch, onMounted, computed } from 'vue'
import NavBar from '../../components/NavBar.vue'
import ClientList from './components/ClientList.vue'
import FooterComp from '../../components/FooterComp.vue'

const clients = ref([])
const activeFilter = ref('all')
const searchQuery = ref('')
//загружаем список клиентов из localstorage
const loadClients = () => {
  const saved = localStorage.getItem('clients')
  if (saved) clients.value = JSON.parse(saved)
}

//отслеживаниею при изменениях и сохранение в localstorage
watch(clients, () => {
  localStorage.setItem('clients', JSON.stringify(clients.value))
}, { deep: true })

onMounted(loadClients)

//метод добавления клиента
const addClient = (newClient) => {
  const newId = clients.value.length > 0
    ? Math.max(...clients.value.map(client => client.id)) + 1
    : 1

  clients.value.push({
    id: newId,
    number: clients.value.length + 1,
    ...newClient
  })
}
//метод редактирования клиентов
const updateClient = (updatedClient) => {
  const index = clients.value.findIndex(client => client.id === updatedClient.id)
  if (index !== -1) {
    clients.value[index] = updatedClient
  }
}

const searchClient = (query) => {
  searchQuery.value = query.toLowerCase()
}

//метод фильтрации списка
const filteredClients = computed(() => {
  let result = [...clients.value]
  
  // Фильтрация по статусу
  if (activeFilter.value === 'present') {
    result = result.filter(client => client.isPresent)
  } else if (activeFilter.value === 'absent') {
    result = result.filter(client => !client.isPresent)
  }
  
  // Фильтрация по поиску
  if (searchQuery.value) {
    result = result.filter(client => 
      client.name.toLowerCase().includes(searchQuery.value))
  }
  
  return result
})

//устанавливаем активный фильтр
const setActiveFilter = (filter) => {
  activeFilter.value = filter
}
</script>

<style lang="scss" scoped>
.container {
    display: flex;
    flex-direction: column;
    min-height: 100vh;
    .main-content {
        flex: 1;
    }
}

</style>