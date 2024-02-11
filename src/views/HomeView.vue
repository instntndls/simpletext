<script setup>
import Card from "@/components/Card.vue";
import ActionBarButton from '@/components/ActionBarButton.vue'
import axios from 'axios'
import { onMounted, ref } from 'vue'

const note = ref([])

const fetchNotes = async () => {
  try {
    const { data } = await axios.get(
      `https://75ab9d6c19df7404.mokky.dev/notes`
    );
    note.value = data
  } catch (err) {
    alert(err)
  }
}
const createNote = async (name, content, date) => {
  try {
    const { data } = await axios.post(
      `https://75ab9d6c19df7404.mokky.dev/notes`,
      {
        name: name,
        content: content,
        date: date
      }
    );
    await fetchNotes()
  } catch (err) {
    alert(err)
  }
}

onMounted(  () => {
  fetchNotes()
})
</script>

<template>
  <div class="MainSection h-screen w-screen flex items-center justify-center bg-white bg-opacity-30 backdrop-blur-2xl">
    <div class="NoteList py-2 gap-4 h-full flex flex-col items-center justify-start bg-black bg-opacity-20 border border-gray-700 border-opacity-30 min-w-48 max-w-96 overflow-auto resize-x px-[10px] w-1/3">
      <div class="ActionBar inline-flex space-x-2.5 items-center justify-between w-full bg-white bg-opacity-0">
        <action-bar-button variant="Default" class="opacity-80">
          <img src="/src/assets/trash.svg" alt="Delete">
        </action-bar-button>
        <action-bar-button variant="Default" @click="createNote( 'Untitled ' + note.length, '', new Date().toLocaleDateString())">
          <img src="/src/assets/new.svg" alt="Add" class="opacity-70">
        </action-bar-button>
      </div>
      <Card v-for="i in note" :key="i" variant="Default" :name="i.name" :date="new Date().toLocaleDateString()"/>
    </div>
    <div class="NoteContent h-full w-full inline-flex flex-col items-start justify-start pb-2.5 bg-black bg-opacity-30">
      <div class="TitleBar h-8 w-full inline-flex items-center justify-end bg-gray-700 bg-opacity-0">
        <div class="Draggable bg-white bg-opacity-0 h-8 w-full"></div>
        <div class="Buttons flex w-[96px]">
          <div class="Hide w-8 h-8 rounded-lg flex items-center justify-center hover:bg-slate-200 hover:bg-opacity-50 transition duration-[50ms]">
            <img src="/src/assets/hide_icon.svg" alt="Hide"/>
          </div>
          <div class="Maximise w-8 h-8 rounded-lg flex items-center justify-center hover:bg-slate-200 hover:bg-opacity-50 transition duration-[50ms]">
            <img src="/src/assets/max_icon.svg" alt="Maximise">
          </div>
          <div class="Minimise w-8 h-8 rounded-lg flex items-center justify-center hover:bg-red-500 transition duration-[50ms]">
            <img src="/src/assets/close_icon.svg" alt="Close">
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style>

</style>
