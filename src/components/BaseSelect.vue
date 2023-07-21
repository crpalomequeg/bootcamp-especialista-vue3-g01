<template>
    <label v-if="label">{{label}}</label>      
    <select 
    :value = "modelValue"
    v-bind="{
        ...$attrs,
        onChange: ($event) => {
            $emit('update:modelValue', $event.target.value)
        }
        }"
    >
    <option value="">Select a {{label}}...</option>
    <option
        v-for="opcion in opciones" 
        :value="opcion.id"
        :key="opcion.id"
        :selected="opcion.id === modelValue"
    >
    {{opcion.descripcion}}
    </option>
    </select>    
</template>

<script setup>
const props = defineProps({
  label: {
    type: String,
    default: ''
  },
  modelValue: {
    type: [String, Number],
    default: ''
  },
  opciones: {
    type: Array,
    required: true
  }
})
</script>