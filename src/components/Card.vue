<script setup>
import { inject, ref } from 'vue'

const fetchNotes = inject('fetchNotes')
const updateNote = inject('updateNote')
const menuOpened = inject('menuOpened')

const props = defineProps({
  id: Number,
  variant: String,
  name: String,
  content: String,
  date: String,
})

const editing = ref(false)
const editText = ref(props.name)
const edit = () => {
  editing.value = !editing.value
  if (!editing.value) {
    fetchNotes()
  }
}

</script>

<template>
  <div
    :class="variant"
    @mousedown.self="
      variant = 'Active';
    "
    @mouseup="variant = 'Hover'"
    @mouseleave="
      variant = 'Default'"
    @mouseover="variant = 'Hover'"
    @click.self="menuOpened = false"
  >
    <div class="flex flex-row justify-between items-center w-full">
      <p v-if="!editing" class="text-sm font-semibold text-white font-['Montserrat']" @click="menuOpened = false">{{ name }}</p>
      <input
        v-if="editing"
        class="w-3/4 border-1 px-2 border-gray-300 rounded-md focus:outline-none font-['Montserrat'] font-medium"
        type="text"
        v-model="editText"
        @mouseleave="edit(); updateNote(props.id, editText, props.content, props.date); fetchNotes()"
        @keydown.self.enter="edit(); updateNote(props.id, editText, props.content, props.date); fetchNotes()"
        autofocus
        tabindex="-1"
        maxlength="16"
      />
      <img
        src="../assets/edit.svg"
        alt="Edit"
        class="opacity-60 size-5 hover:opacity-100"
        v-if="variant === 'Hover' || variant === 'Active'"
        @click="edit()"
      />
    </div>
    <p class="text-sm text-white font-['Montserrat']" @click="menuOpened = false">{{ date }}</p>
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
  select-none;
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
  transition duration-[50ms];
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
  transition duration-[25ms];
}
</style>
