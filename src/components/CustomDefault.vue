<script setup>
import { onMounted, ref, computed } from 'vue'
const count = ref(0);
defineProps({
  msg: {
    type: String,
    required: true
  }
})
defineEmits(['']);
onMounted(()=>{});
const computedDataAPI = computed((item)=>{ return item });
</script>

<template>
  <div class="greetings">
    <h1 class="green">{{ msg }}</h1>
  </div>
</template>

<style scoped>
</style>
