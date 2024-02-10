<script setup>
const props = defineProps({
  variant: String,
  name: String,
  date: String
})

function say() {
  alert(props.name);
}
</script>

<template>
  <div :class="variant" @click="say" @mousedown="variant='Active'" @mouseup="variant='Default'" @mouseleave="variant='Default'">
    <p class="text-sm font-semibold text-white">{{ name }}</p>
    <p class="text-sm text-white">{{ date }}</p>
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
  hover:border-opacity-30
  border-2 border-opacity-0
  border-white
  cursor-pointer
  select-none
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
}
</style>