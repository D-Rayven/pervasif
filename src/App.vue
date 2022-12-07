<template>
  <div id="app">
    <div class="cell cell-map">
      <map-component />
    </div>
  </div>
</template>

<script>
  import MapComponent from './components/MapComponent.vue';
  export default {
  components: { MapComponent },
    name: 'App'
  }
</script>

<style>
  html, body {
    height: 100%;
    margin: 0;
  }

  #app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    height: 100%; 
    display: grid;
    grid-template-columns: 100vh;
    grid-auto-rows: 1fr;
    grid-gap: 1rem;
    padding: 1rem;
    box-sizing: border-box;
  }

  .cell {
    border-radius: 4px;
    background-color: lightgrey;
  }
</style>