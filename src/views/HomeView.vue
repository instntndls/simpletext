<script setup>
import Card from '@/components/Card.vue'
import ActionBarButton from '@/components/ActionBarButton.vue'
import axios from 'axios'
import { onMounted, ref, provide, watch } from 'vue'
import NullPlaceholder from '@/components/NullPlaceholder.vue'

const note = ref(JSON.parse(localStorage.getItem('notes')) || []);
const selectedNote = ref(null)
const content = ref('')
const title = ref('')
const menuOpened = ref(true)
const mobile = ref()

const createDate = () => {
  return new Date().toLocaleDateString() + ' ' + new Date().getHours() + ':' + new Date().getMinutes()
}

const updateLocalStorage = (notes) => {
  localStorage.setItem('notes', JSON.stringify(notes));
}

function fetchNotes() {
  // No need to fetch from an API, just get the notes from local storage
  note.value = JSON.parse(localStorage.getItem('notes')) || [];
  if (note.value.length <= 0) {
    selectedNote.value = null
  }
}

const updateNote = (id, name, content, date) => {
  const notes = JSON.parse(localStorage.getItem('notes')) || [];
  const updatedNotes = notes.map(note => {
    if (note.id === id) {
      return {
        id,
        name,
        content,
        date
      };
    }
    return note;
  });
  updateLocalStorage(updatedNotes);
  fetchNotes()
};

const createNote = (name, content, date) => {
  const notes = JSON.parse(localStorage.getItem('notes')) || [];
  const newNote = {
    id: note.value.length > 0 ? note.value[note.value.length - 1].id + 1 : 1,
    name,
    content,
    date
  };
  const updatedNotes = [...notes, newNote];
  updateLocalStorage(updatedNotes);
  fetchNotes()
};

const deleteNote = (id) => {
  const notes = JSON.parse(localStorage.getItem('notes')) || [];
  const updatedNotes = notes.filter(note => note.id !== id);
  updateLocalStorage(updatedNotes);
  fetchNotes()
};

onMounted(() => {
  mobile.value = window.innerWidth < 768;
  window.addEventListener('resize', () => {
    if (window.innerWidth < 768) {
      mobile.value = true
      menuOpened.value = false
      console.log(menuOpened.value, mobile.value)
    }
    else {
      mobile.value = false
      menuOpened.value = true
      console.log(menuOpened.value, mobile.value)
    }
  })
})

provide('fetchNotes', fetchNotes)
provide('updateNote', updateNote)
provide('menuOpened', menuOpened)
</script>

<template>
  <div
    class="MainSection h-screen w-screen flex items-center justify-center bg-white bg-opacity-30 backdrop-blur-2xl resize-none"
  >
    <div
      :class="menuOpened && !mobile ? 'NoteList' : 'hidden'"
    >
      <div
        class="ActionBar inline-flex space-x-2.5 items-center justify-between w-full bg-white bg-opacity-0"
      >
        <div class="DeleteButton w-10 h-8">
          <action-bar-button variant="Default" @click="deleteNote(selectedNote)" class="opacity-80">
            <img src="/src/assets/trash.svg" alt="Delete" />
          </action-bar-button>
        </div>
        <div class="CreateButton w-10 h-8">
          <action-bar-button
            variant="Default"
            @click="createNote('Note ' +(note.length + 1), '', createDate())"
          >
            <img src="/src/assets/new.svg" alt="Add" class="opacity-70" />
          </action-bar-button>
        </div>

      </div>
      <Card
        v-for="i in note"
        :id="i.id"
        variant="Default"
        :name="i.name"
        :content="i.content"
        :date="i.date"
        @click="selectedNote = i.id; content = i.content; title = i.name;
        !mobile ? menuOpened = true : menuOpened = false;"
      />
    </div>
    <div v-if="menuOpened && mobile" class="NoteListMobile">
      <div class="fixed top-0 w-full h-24 p-4 bg-neutral-800">
        <action-bar-button
          variant="Default"
          @click="createNote('Note ' +(note.length + 1), '', createDate()); selectedNote = 1"
        >
          <img src="/src/assets/new.svg" alt="Add" class="opacity-70"/>
        </action-bar-button>
      </div>
      <div class="mt-16"></div>
      <Card
        v-for="i in note"
        :id="i.id"
        variant="Default"
        :name="i.name"
        :content="i.content"
        :date="i.date"
        @click="selectedNote = i.id; content = i.content; title = i.name"
      />
      <NullPlaceholder v-if="note.length <= 0" class="mt-24"/>
    </div>
    <div
      class="NoteContent h-full w-full inline-flex flex-col items-start justify-start bg-black bg-opacity-40 pt-6 pl-2"
    >
      <div v-if="selectedNote" class="flex items-center Title text-white text-3xl select-none font-['Montserrat'] font-semibold pb-6">
        <div :class="menuOpened ? 'hidden' : 'BackButton'" @click="menuOpened = true">
          <img class="w-full h-full fill-white" src="/src/assets/arrow-back.svg" alt="Back">
        </div>
        <p class="h-min pl-4">{{title}}</p>
        <div :class="menuOpened ? 'hidden' : 'BackButton right-4 absolute size-4'" @click="menuOpened = true; deleteNote(selectedNote)">
          <img class="w-full h-full fill-white" src="/src/assets/trash.svg" alt="Back">
        </div>
      </div>
      <textarea v-if="selectedNote!==null" type="text" spellcheck="false" v-model="content"
                @input="updateNote(selectedNote, title, content, createDate()); fetchNotes()"
                class="w-full h-full bg-transparent border-b-2 border-transparent outline-none text-white px-4 resize-none caret-white font-['Montserrat'] font-light text-xl" />
      <NullPlaceholder v-else/>
    </div>
  </div>
</template>

<style>
.NoteList {
  @apply py-2 gap-4 h-full flex flex-col
  items-center justify-start bg-black
  bg-opacity-30 border border-gray-700
  border-opacity-30 min-w-48 max-w-96
  overflow-auto resize-x px-[10px] w-1/3
}
.NoteListMobile {
  @apply fixed gap-4 px-4 py-6 h-full flex flex-col
  items-center justify-start bg-neutral-800
  overflow-auto w-full
}
.BackButton {
  @apply size-10 z-50
  flex items-center justify-center
  cursor-pointer p-2
}
</style>
