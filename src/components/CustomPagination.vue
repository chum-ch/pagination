<script setup>
import { onMounted, ref, computed } from 'vue'
defineProps({
  msg: {
    type: String,
    required: false
  }
})
// defineEmits(['']);
const data = ref([])
onMounted(async () => {
  try {
    const response = await fetch('https://restcountries.com/v3.1/all')
    const jsonData = await response.json()
    data.value = jsonData
  } catch (error) {
    console.error(error)
  }
})
const currentPage = ref(1)
const rowDataPerPage = 10
const numberPage = 9
const paginatedData = computed(() => {
  const startIndex = (currentPage.value - 1) * rowDataPerPage
  const endIndex = startIndex + rowDataPerPage
  return data.value.slice(startIndex, endIndex)
})

const totalPages = computed(() => Math.ceil(data.value.length / rowDataPerPage))

const pageRange = computed(() => {
  let rangeStart = Math.max(currentPage.value - 2, 1)
  const rangeEnd = Math.min(rangeStart + numberPage, totalPages.value)

  if (rangeEnd - rangeStart < numberPage) {
    rangeStart = Math.max(rangeEnd - numberPage, 1)
  }

  const range = []
  for (let i = rangeStart; i <= rangeEnd; i++) {
    range.push(i)
  }
  return range
})
function nextPage() {
  if (currentPage.value < totalPages.value) {
    currentPage.value++
  }
}

function prevPage() {
  if (currentPage.value > 1) {
    currentPage.value--
  }
}
function goToPage(page) {
  currentPage.value = page
}
</script>

<template>
  <main>
    <h1>
      Catalog country
    </h1>
    <table>
    <tr>
      <th>Name's flag</th>
      <th>Name's country</th>
      <th>Population</th>
      <th>Area</th>
      <th>Region</th>
      <th>Subregion</th>
    </tr>
    <tr
    v-for="item in paginatedData" :key="item.name"
    >
      <td> 
        <img :src="item.flags.png" alt="" width="45">
      </td>
      <td> {{ item.name.official }}</td>
      <td> {{ item.population }}</td>
      <td> {{ item.area }}</td>
      <td> {{ item.region }}</td>
      <td> {{ item.subregion }}</td>
    </tr>
  </table>

    <div class="pagination">
      <button @click="prevPage" :disabled="currentPage === 1">Previous</button>
      <span v-for="page in pageRange" :key="page">
        <button @click="goToPage(page)" :class="{ active: page === currentPage }">
          {{ page }}
        </button>
      </span>
      <button @click="nextPage" :disabled="currentPage === totalPages">Next</button>
    </div>
  </main>
</template>

<style scoped>
.pagination {
  text-align: center;
  position: fixed;
  bottom: 10px;
  right: 0;
  left: 0;
}
.pagination button {
  padding: 8px;
  margin: 0 5px;
  background: var(--btn-bg);
  color: var(--btn-hover-bg);
  font-weight: bold;
  outline: none;
  border: none;
  border-radius: 5px;
}
.pagination .active {
  background: var(--btn-hover-bg);
  color: var(--vt-c-white);
}
.pagination button:hover {
  color: var(--vt-c-white);
  background: var(--btn-hover-bg);
}
 /* CSS styles for the table */
 table {
      border-collapse: collapse;
      width: 100%;
    }
    tr th {
      font-weight: bold;
    }
    th, td {
      border: 1px solid black;
      padding: 8px;
      text-align: left;
    }

    th {
      background-color: #f2f2f2;
    }
    h1 {
      text-align: center;
      font-weight: bolder;
      background: linear-gradient(to top, #ff7f00, #ff0080, var(--btn-hover-bg));
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
    }
</style>