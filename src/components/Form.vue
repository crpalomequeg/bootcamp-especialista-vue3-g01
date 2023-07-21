<script setup>
import { ref, watch } from 'vue'
import { createHero, updateHero } from '@/services/heroes'

import { createToaster } from "@meforma/vue-toaster"
import BaseInput from './BaseInput.vue';
import BaseSelect from './BaseSelect.vue';
import BaseButton from './BaseButton.vue';

const toaster = createToaster()

const props = defineProps({
  hero: {
    type: Object,
    default: {}
  }
})

const emit = defineEmits(['onRefreshAfterUpdate'])

watch(() => props.hero, (newHero, oldHero) => {
  console.log(newHero, oldHero)

  formData.value = {
    id: newHero.id,
    name: newHero.name,
    realName: newHero.real_name,
    gender: newHero.gender,
    birth: newHero.birth,
  }
})

const formData = ref({
  id: '',

  name: '',
  realName: '',
  gender: '',
  birth: '',

  aliases: 'aliases',
  deck: 'deck description',
  image_screen_large_url: 'url_large',
  image_screen_url: 'url'
})

const handleForm = async (event) => {
  // TODO: Limpiar el formulario después de crear o actualizar
  // TODO: Actualizar la lista de heroes después de actualizar y permanecer en la misma página

  try {

    const newHero = formData.value

    newHero.gender = Number(newHero.gender)

    newHero['real_name'] = newHero.realName

    delete newHero.realName

    const isANewHero = !Boolean(newHero.id)

    console.log(isANewHero, newHero.id)

    if (isANewHero) {
      const res = await createHero({ form: newHero })

      if (res) {
        toaster.success(`Se creó el heroe correctamente`);
      }
    } else {
      console.log('Updating...')

      const res = await updateHero({ form: newHero })

      if (res) {
        toaster.success(`Se actualizó el heroe correctamente`);
      }

      emit('onRefreshAfterUpdate')
    }

    formData.value = {
      id: '',

      name: '',
      realName: '',
      gender: '',
      birth: '',

      aliases: 'aliases',
      deck: 'deck description',
      image_screen_large_url: 'url_large',
      image_screen_url: 'url'
    }

    // DONE: Validar la respuesta y mostrar un mensaje de exito o de error
  } catch (error) {
    console.log(error)
    
    toaster.error(`Hubo un error, intentalo denuevo más tarde`);
  }
}

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
  <!-- TODO: Hacer que los componentes del formulario y cualquier parte de la app (input, select, buttons) sean reutilizables usando @input, $attrs. Tal como está en los videos del Canvas. Ejemplo: <BaseInput />, <BaseSelect />, <BaseButton />, etc -->
  <h2>New Hero</h2>
  <form @submit.prevent="handleForm">

    <BaseInput
      v-model="formData.name"
      label="Name"
      placeholder="Ex. Spiderman"
      type="text"
    />
    
    <BaseInput
      v-model="formData.realName"
      label="Real Name"
      placeholder="Ex. Spiderman"
      type="text"
    />

    <BaseSelect
      :opciones="genders"
      v-model="formData.gender"
      label="Gender"
    />

    <BaseInput
      v-model="formData.birth"
      label="Birth"
      placeholder=""
      type="date"
    />

    <BaseButton
      type="submit"
      label="Save"
    />

    <!-- <button type="submit">Save</button> -->

    <pre>{{ hero }}</pre>
  </form>
</template>