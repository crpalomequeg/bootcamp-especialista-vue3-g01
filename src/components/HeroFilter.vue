<script setup>
  import { ref } from 'vue'
  import BaseInput from './BaseInput.vue';
  import BaseSelect from './BaseSelect.vue';

  const query = ref('')
  const filterByGender = ref('')

  const emit = defineEmits(['onSearch', 'onFilter'])

  const genders = [ 
  {
  "id": "1",
  "descripcion": "Male",
  },
  {
  "id": "2",
  "descripcion": "Female",
  },
  {
  "id": "0",
  "descripcion": "Other",
  }
  ]

</script>

<template>
  <div class="grid">
    <div>
      <BaseInput
        v-model="query"
        placeholder="Search by name"
        type="text"
        @keyup="emit('onSearch', query)"
      />

      <!-- <input
        type="text"
        placeholder="Search by name"
        v-model="query"
        @keyup="emit('onSearch', query)"
      /> -->
    </div>
    <div>
      <!-- <select v-model="filterByGender" @change="emit('onFilter', filterByGender)">
        <option value="" selected>Select a gender…</option>
        <option value="1">Male</option>
        <option value="2">Female</option>
        <option value="0">Other</option>
      </select> -->

      <BaseSelect
        :opciones="genders"
        v-model="filterByGender"
        label="Gender"
        :@change="emit('onFilter', filterByGender)"
      />

    </div>
  </div>
</template>