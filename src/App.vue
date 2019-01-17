<template>
  <div id="app">
    <GameTable />
  </div>
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator';
import GameTable from './components/GameTable.vue';

@Component({
  components: {
    GameTable,
  },
})
export default class App extends Vue { }
</script>

<style>
html, body, #app {
  width: 100%;
  height: 100%;
  box-sizing: border-box;
}

* {
  box-sizing: inherit;
}

body {
  background: #2c3e50;
  color: aliceblue;
  margin: 0;
  overflow-x: hidden;
}

#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  padding-top: 80px;
}
</style>
