<template>    
    <label v-if="label">{{ label }}</label>      
    <input
    v-bind="$attrs"
    :type = "type"
    :placeholder = "placeholder"
    required
    :value = "modelValue"
    @input = "$emit('update:modelValue', $event.target.value)"
    />    
</template>

<script setup>
const props = defineProps({
  type: {
    type: String,
    default: 'text'
  },
  label: {
    type: String,
    default: ''
  },
  placeholder: {
    type: String,
    default: ''
  },
  modelValue: {
    type: [String, Number, Date],
    default: ''
  }
})
</script>