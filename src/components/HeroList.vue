<script setup>
import { ref } from 'vue'

import IconGenderMale from '../components/icons/otro/IconGenderMale.vue'
import IconGenderFemale from '@/components/icons/IconGenderFemale.vue'

const page = ref(1)
const limit = ref(4)
const count = ref(100)
const name = ref('')
const gender = ref('')

const isOpenImageModal = ref(false)

// Vue 2
// export default { 
//   props: {
//     heroes: {
//       type: String,
//       required: true
//     }
//   }
// }

defineProps({
  heroes: {
    type: Array,
    required: true
  }
})

const emit = defineEmits(['onPage','onSearch','xonFilter'])

const firstPage = () => {
  if (page.value === 1) return

  page.value = 1
  name.value = inputSearch.value
  
  emit('onPage', page.value)
}

const nextPage = () => {
  const lastPage = count.value / limit.value

  if (page.value >= lastPage) return

  page.value = page.value + 1
  name.value = inputSearch.value
  
  emit('onPage', page.value)
}

const prevPage = () => {
  if (page.value === 1) return

  page.value = page.value - 1
  name.value = inputSearch.value
  
  emit('onPage', page.value)
}

const lastPage = () => {
  const lastPage = count.value / limit.value // 100 / 4 = 25 pages

  if (page.value === lastPage) return

  page.value = lastPage
  name.value = inputSearch.value

  emit('onPage', page.value)
}

const showImage = (image) => {
  imageModal.src = image
  isOpenImageModal.value = true
}

const toggleImageModal = () => {
  isOpenImageModal.value = false
}

const onSearch = () => {
  console.log(inputSearch.value)
  name.value = inputSearch.value
  emit('onSearch', name.value)
}

const xonFilter = () => {
  // console.log(selectFilter.value)
  gender.value = selectFilter.value
  emit('xonFilter', gender.value)
}

</script>

<template>

  <div class="pagination">
    <input id="inputSearch" type="text" placeholder="Input name to search ..." />
    <button @click="onSearch">Search</button>
  </div>
  <div>
    <select id="selectFilter" @change="xonFilter">
      <option value="">Select a gender to filter ...</option>
      <option value="1">Male</option>
      <option value="2">Female</option>
      <option value="0">Agender</option>
    </select>
  </div>
    
  <table role="grid">
    <thead>
      <tr>
        <th>#</th>
        <th>Image</th>
        <th>Name</th>
        <th>Gender</th>
      </tr>
    </thead>

    <tbody>
      <tr v-for="hero in heroes" :key="hero.id">
        <th>{{ hero.id }}</th>
        <td>
          <a @click="showImage(hero.image_screen_large_url)">
            <img :src="hero.image_screen_url" width="156" height="88" />
          </a>
        </td>
        <td>
          <div>{{ hero.name }}</div>
          <div>Real name: {{ hero.real_name }}</div>
          <div>Aliases: {{ hero.aliases }}</div>
          <div v-if="hero.birth">Birth: {{ hero.birth }}</div>
        </td>
        <th>
          <IconGenderMale v-if="hero.gender === 1" class="gender--male" />
          <IconGenderFemale  v-else-if="hero.gender === 2" class="gender--female" />
          <span v-else>-</span>
        </th>
      </tr>
    </tbody>
  </table>

  <div class="pagination">
    <button @click="firstPage">First</button>
    <button @click="prevPage">Previous</button>
    <button disabled class="contrast outline">{{ page }} of {{ count / limit }}</button>
    <button @click="nextPage">Next</button>
    <button @click="lastPage">Last</button>
  </div>

  <dialog :open="isOpenImageModal">
    <article>
      <img id="imageModal" />
      <footer>
        <a
          href="#cancel"
          role="button"
          class="secondary"
          @click="toggleImageModal">
          Close
        </a>        
      </footer>
    </article>
  </dialog>

</template>

<style scoped>
.gender--male {
  fill: #60a5fa;
}
.gender--female {
  fill: #fb7185;
}
.gender--none {
  fill: #d7ef1c;
}

.pagination {
  display: flex;
  gap: 1rem;
}

.pagination div {
  width: 100px;
}
</style>