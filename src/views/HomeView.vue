<script setup>
import Card from '@/components/Card.vue'
import ActionBarButton from '@/components/ActionBarButton.vue'
import axios from 'axios'
import { onMounted, ref, provide  } from 'vue'

const note = ref([])
const selectedNote = ref(null)
const content = ref('')
const title = ref('')

const createDate = () => {
  return new Date().toLocaleDateString() + ' ' + new Date().getHours() + ':' + new Date().getMinutes()
}

const fetchNotes = async () => {
  try {
    const { data } = await axios.get(`https://75ab9d6c19df7404.mokky.dev/notes`)
    note.value = data
  } catch (err) {
    alert(err)
  }
}

const updateNote = async (id, name, content, date) => {
  await axios.patch(`https://75ab9d6c19df7404.mokky.dev/notes/`+id , {
    id: id,
    name: name,
    content: content,
    date: date
  })
}

const createNote = async (name, content, date) => {
  try {
    await axios.post(`https://75ab9d6c19df7404.mokky.dev/notes`, {
      name: name,
      content: content,
      date: date
    })
    await fetchNotes()
  } catch (err) {
    alert(err)
  }
}

const deleteNote = async (id) => {
  try {
    await axios.delete(`https://75ab9d6c19df7404.mokky.dev/notes/`+id)
    await fetchNotes()
    selectedNote.value = null
  } catch (err) {
    alert("Select a note to delete it!")
  }
}

onMounted(() => {
  fetchNotes()
})

provide('fetchNotes', fetchNotes)
provide('updateNote', updateNote)
</script>

<template>
  <div
    class="MainSection h-screen w-screen flex items-center justify-center bg-white bg-opacity-30 backdrop-blur-2xl"
  >
    <div
      class="NoteList py-2 gap-4 h-full flex flex-col items-center justify-start bg-black bg-opacity-30 border border-gray-700 border-opacity-30 min-w-48 max-w-96 overflow-auto resize-x px-[10px] w-1/3"

    >
      <div
        class="ActionBar inline-flex space-x-2.5 items-center justify-between w-full bg-white bg-opacity-0"
      >
        <action-bar-button variant="Default" @click="deleteNote(selectedNote)" class="opacity-80">
          <img src="/src/assets/trash.svg" alt="Delete" />
        </action-bar-button>
        <action-bar-button
          variant="Default"
          @click="createNote('Note ' + note.length, '', createDate())"
        >
          <img src="/src/assets/new.svg" alt="Add" class="opacity-70" />
        </action-bar-button>
      </div>
      <Card
        v-for="i in note"
        :id="i.id"
        variant="Default"
        :name="i.name"
        :content="i.content"
        :date="i.date"
        @click="selectedNote = i.id; content = i.content; title = i.name;"
      />
    </div>
    <div
      class="NoteContent h-full w-full inline-flex flex-col items-start justify-start bg-black bg-opacity-40"
    >
      <div
        class="TitleBar h-8 w-full inline-flex items-center justify-end bg-gray-700 bg-opacity-0"
      >
        <div class="Draggable bg-white bg-opacity-0 h-8 w-full"></div>
        <div class="Buttons flex w-[96px]">
          <div
            class="Hide w-8 h-8 rounded-lg flex items-center justify-center hover:bg-slate-200 hover:bg-opacity-50 transition duration-[50ms]"
          >
            <img src="/src/assets/hide_icon.svg" alt="Hide" />
          </div>
          <div
            class="Maximise w-8 h-8 rounded-lg flex items-center justify-center hover:bg-slate-200 hover:bg-opacity-50 transition duration-[50ms]"
          >
            <img src="/src/assets/max_icon.svg" alt="Maximise" />
          </div>
          <div
            class="Minimise w-8 h-8 rounded-lg flex items-center justify-center hover:bg-red-500 transition duration-[50ms]"
          >
            <img src="/src/assets/close_icon.svg" alt="Close" />
          </div>
        </div>
      </div>
      <div v-if="selectedNote!=null" class="Content h-full w-full">
        <div class="Title text-white text-3xl pl-6 pt-4 select-none font-['Montserrat'] font-semibold" >{{title}}</div>
        <textarea type="text" spellcheck="false" v-model="content" @input="updateNote(selectedNote, title, content, createDate()); fetchNotes()" class="w-full h-full bg-transparent border-b-2 border-transparent outline-none text-white p-6 resize-none caret-white font-['Montserrat'] font-light text-xl"/>
      </div>
      <div v-else class="h-full w-full flex flex-col items-center justify-center gap-6 pb-64">
        <p class="text-8xl font-['Montserrat']">🥺</p>
        <p class="text-3xl font-['Montserrat'] font-bold text-white">You haven't selected any notes!</p>
        <p class="text-1xl font-['Montserrat'] font-semibold text-white">Select a note from the list to view it</p>
      </div>
    </div>
  </div>
</template>

<style>

</style>
