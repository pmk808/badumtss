<template>
  <div id="app">
    <h1>Web Drum App</h1>
    <DrumPad />
  </div>
</template>

<script setup>
import DrumPad from './components/DrumPad.vue';
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>