<template>
    <button 
    v-bind="$attrs"
    :type="type"
    >{{label}}</button>
</template>

<script setup>
const props = defineProps({
  type: {
    type: String,
    default: 'button'
  },
  label: {
    type: String,
    default: ''
  }
})
</script>