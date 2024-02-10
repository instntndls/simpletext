<script setup>
import Card from "@/components/Card.vue";


</script>

<template>
  <div class="MainSection h-screen w-screen flex items-center justify-center bg-white bg-opacity-30 backdrop-blur-2xl">
    <div class="NoteList py-2 gap-4 h-full flex flex-col items-center justify-start bg-black bg-opacity-20 border border-gray-700 border-opacity-30 min-w-48 max-w-96 overflow-auto resize-x px-[10px]">
      <div class="ActionBar inline-flex space-x-2.5 items-center justify-between w-full bg-white bg-opacity-0">
<!--        ActionBarButtons-->
      </div>
      <Card v-for="i in 10" :key="i" variant="Default" :name="'Note ' + i" :date="new Date().toLocaleDateString()" />
    </div>
    <div class="NoteContent h-full w-full inline-flex flex-col items-start justify-start pb-2.5 bg-black bg-opacity-30">
      <div class="TitleBar h-8 w-full inline-flex items-center justify-end bg-gray-700 bg-opacity-0">
        <div class="Draggable bg-white bg-opacity-0 h-8 w-full"></div>
        <div class="Buttons flex w-[96px]">
          <div class="Hide w-8 h-8 rounded-lg flex items-center justify-center hover:bg-slate-200 hover:bg-opacity-50">
            <img src="/src/assets/hide_icon.svg" alt="Hide"/>
          </div>
          <div class="Maximise w-8 h-8 rounded-lg flex items-center justify-center hover:bg-slate-200 hover:bg-opacity-50">
            <img src="/src/assets/max_icon.svg" alt="Maximise">
          </div>
          <div class="Minimise w-8 h-8 rounded-lg flex items-center justify-center hover:bg-red-500">
            <img src="/src/assets/close_icon.svg" alt="Close">
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style>

</style>
