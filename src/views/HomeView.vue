<script setup>
import Card from '@/components/Card.vue'
import ActionBarButton from '@/components/ActionBarButton.vue'
import axios from 'axios'
import { onMounted, ref, provide  } from 'vue'
import { v4 as uuidv4 } from 'uuid';

const note = ref(JSON.parse(localStorage.getItem('notes')) || []);
const selectedNote = ref(null)
const content = ref('')
const title = ref('')

const createDate = () => {
  return new Date().toLocaleDateString() + ' ' + new Date().getHours() + ':' + new Date().getMinutes()
}

const updateLocalStorage = (notes) => {
  localStorage.setItem('notes', JSON.stringify(notes));
}

function fetchNotes() {
  // No need to fetch from an API, just get the notes from local storage
  note.value = JSON.parse(localStorage.getItem('notes')) || [];
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
          @click="createNote('Note ' +(note.length + 1), '', createDate())"
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
        @click="selectedNote = i.id; content = i.content; title = i.name; "
      />
    </div>
    <div
      class="NoteContent h-full w-full inline-flex flex-col items-start justify-start bg-black bg-opacity-40"
    >
      <div
        class="TitleBar h-8 w-full inline-flex items-center justify-end bg-gray-700 bg-opacity-0">
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
      <div v-if="selectedNote" class="Title text-white text-3xl pl-6 pt-4 select-none font-['Montserrat'] font-semibold">{{title}}</div>
      <textarea v-if="selectedNote" type="text" spellcheck="false" v-model="content"
                @input="updateNote(selectedNote, title, content, createDate()); fetchNotes()"
                class="w-full h-full bg-transparent border-b-2 border-transparent outline-none text-white p-6 resize-none caret-white font-['Montserrat'] font-light text-xl" />
      <div v-else class="h-full w-full flex flex-col items-center justify-center gap-6 pb-64 text-center">
        <p class="text-8xl font-['Montserrat']">🥺</p>
        <p class="text-3xl font-['Montserrat'] font-bold text-white">You haven't selected or created any notes!</p>
        <p class="text-1xl font-['Montserrat'] font-semibold text-white">Create or select a note from the list to view it</p>
      </div>
    </div>
  </div>
</template>

<style>

</style>
