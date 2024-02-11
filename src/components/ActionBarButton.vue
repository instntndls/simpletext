<script setup>
const props = defineProps({
  variant: String
})

</script>

<template>
  <div :class="variant" @mousedown="variant='Active'" @mouseup="variant='Default'" @mouseleave="variant='Default'">
    <slot/>
  </div>
</template>

<style scoped>
.Default {
  @apply inline-flex flex-col
  space-y-2.5 items-center
  justify-center p-2
  bg-white bg-opacity-0
  rounded w-8 h-6
  hover:border-opacity-50
  hover:scale-105
  border-2 border-opacity-30
  border-white
  cursor-pointer
  select-none
  transition duration-[50ms]
}
.Active {
  @apply inline-flex flex-col
  space-y-2.5 items-center
  justify-center p-2
  bg-white bg-opacity-60
  rounded w-8 h-6
  border-2 border-opacity-0
  border-white
  cursor-pointer
  select-none
  transition duration-[50ms]
}
</style>