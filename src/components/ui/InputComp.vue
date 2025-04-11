<template>

    <input
      v-if="type === 'text'"
      type="text"
      :placeholder="placeholderText"
      :value="modelValue"
      @input="$emit('update:modelValue', $event.target.value)"
    >
  

    <label v-else-if="type === 'checkbox'" class="checkbox-wrapper">
      <input
        type="checkbox"
        :checked="modelValue"
        @change="$emit('update:modelValue', $event.target.checked)"
      >
    </label>

    
    <select
        v-else-if="type === 'select'"
        :value="modelValue"
        @change="$emit('update:modelValue', $event.target.value)"
        class="select-input"
    >
        <option value="Прохожий">Прохожий</option>
        <option value="Клиент">Клиент</option>
        <option value="Партнер">Партнер</option>
    </select>
  </template>
  
<script setup>
defineProps({
  type: {
    type: String,
    default: 'text',
    validator: value => ['text', 'checkbox', 'select'].includes(value)
  },
  placeholderText: {
    type: String,
    default: ""
  },
  modelValue: {
    type: [String, Boolean, Number],
    default: ""
  },
  options: {
    type: Array,
    default: () => []
  }
})
  
defineEmits(['update:modelValue'])
</script>
  
<style lang="scss" scoped>
@use '/src/assets/css/root.scss' as *;

input[type="text"], select {
  box-shadow: 0px 1px 4px 0px #00000029;
  border-radius: 5px;
  border: 1px solid transparent;
  outline: none;
  max-width: 394px;
  width: 100%;
  height: 52px;
  padding: 15px;
  font-size: 16px;
  &::placeholder {
    color: $placeholder-text;
    font-size: 16px;
  }
    
  &:focus {
    border: 1px solid $border-color;
  }
}
  
.checkbox-wrapper {
  display: flex;
  align-items: center;
  gap: 8px;
  cursor: pointer;
  
  input[type="checkbox"] {
    width: 18px;
    height: 18px;
    margin: 0;
  }
}
  
.select-input {
  appearance: none;
  background-repeat: no-repeat;
  background-position: right 10px center;
  background-size: 16px;
  padding-right: 30px;
}
</style>