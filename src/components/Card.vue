<script setup>

import { ref } from 'vue'
import axios from 'axios'

const props = defineProps({
  key: Number,
  variant: String,
  name: String,
  date: String
})
const editing = ref(false)
const editText = ref(props.name)
const edit = (text) => {
  editing.value = !editing.value
}



</script>

<template>
  <div :class="variant" @mousedown.self="variant='Active'; editing = false" @mouseup="variant='Hover'" @mouseleave="variant='Default'; editing = false" @mouseover="variant='Hover'">
    <div class="flex flex-row justify-between items-center w-full">
      <p v-if="!editing" class="text-sm font-semibold text-white font-['Montserrat']">{{ name }}</p>
      <input v-if="editing" class="border-1 px-2 border-gray-300 rounded-md focus:outline-none font-['Montserrat'] font-medium" type="text" v-model="editText" @keydown.self.enter="edit(editText)">
      <img src="../assets/edit.svg" alt="Edit" class="opacity-60 size-5 hover:opacity-100" v-if="variant==='Hover' || variant==='Active'" @click="editing = !editing">
    </div>
    <p class="text-sm text-white font-['Montserrat']">{{ date }}</p>
  </div>
</template>

<style scoped>
.Default {
  @apply inline-flex flex-col
  space-y-2.5 items-start
  justify-start
  px-4 py-2.5
  bg-white bg-opacity-0
  rounded w-full
  border-2 border-opacity-0
  border-white
  cursor-pointer
  select-none
}
.Hover {
  @apply inline-flex flex-col
  space-y-2.5 items-start
  justify-start
  px-4 py-2.5
  bg-white bg-opacity-0
  rounded w-full
  hover:border-opacity-30
  hover:scale-[1.03]
  border-2 border-opacity-0
  border-white
  cursor-pointer
  select-none
  transition duration-[50ms]
}
.Active {
  @apply inline-flex flex-col
  space-y-2.5 items-start
  justify-start
  px-4 py-2.5
  bg-white bg-opacity-30
  rounded w-full
  border-2 border-opacity-0
  border-white
  cursor-pointer
  select-none
  scale-[1.03]
  transition duration-[25ms]
}
</style>