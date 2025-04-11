<template>
  <div class="footer">
    <h4>Фильтровать по:</h4>
    <FilterItem>
      <template #default>
        <a 
          href="#" 
          class="filter-link" 
          :class="{ 'active': activeFilter === 'absent' }"
          @click.prevent="setActiveFilter('absent')"
        >
            Отсутствующим
        </a>
        <a
          href="#" 
          class="filter-link" 
          :class="{ 'active': activeFilter === 'present' }"
          @click.prevent="setActiveFilter('present')"
        >
            Присутствующим
        </a>
        <a
            href="#" 
          class="filter-link" 
          :class="{ 'active': activeFilter === 'all' }"
          @click.prevent="setActiveFilter('all')"
        >
            Все
        </a>
      </template>
    </FilterItem>
  </div>
</template>

<script setup>
import FilterItem from './ui/FilterItem.vue'

const props = defineProps({
  activeFilter: String
})

const emit = defineEmits(['update:activeFilter'])
//передаем через эмит активный фильтр
const setActiveFilter = (filter) => {
    emit('update:activeFilter', filter)
}
</script>

<style lang="scss" scoped>
@use '/src/assets/css/root.scss' as *;

.footer {
  display: flex;
  flex-wrap: wrap;
  align-items: center;
  gap: 20px;
  padding: 15px;
  
  @media (max-width: 768px) {
    flex-direction: column;
    align-items: flex-start;
    gap: 12px;
  }
}

.filter-label {
  margin: 0;
  white-space: nowrap;
  font-size: 1rem;
  
  @media (max-width: 480px) {
    font-size: 0.9rem;
  }
}

.filter-links {
  display: flex;
  flex-wrap: wrap;
  gap: 10px;
  
  @media (max-width: 480px) {
    gap: 8px;
  }
}

.filter-link {
  display: inline-block;
  padding: 8px 16px;
  border-radius: 20px;
  color: $second-text-color;
  text-decoration: none;
  transition: all 0.3s ease;
  font-size: 0.9rem;
  white-space: nowrap;
  font-weight: 600;
  
  &:hover {
    background-color: $filter-hover;
  }
  
  &.active {
    background-color: $filter-pick;
    color: $white-text-color;
    font-weight: 500;
  }
  
  @media (max-width: 480px) {
    padding: 6px 12px;
    font-size: 0.85rem;
  }
}
</style>