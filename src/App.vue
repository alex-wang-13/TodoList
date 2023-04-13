<template>
  <div id="app">
    <!--img alt="Vue logo" src="https://vuejs.org/images/logo.png" /-->
    <!--HelloWorld msg="Welcome to Your Vue.js App"/-->
    <!--TestComponent msg="Who is Madhu Auntie?" /-->
    <!--br-->
    <Table />
  </div>
</template>

<script>
// import HelloWorld from './components/HelloWorld.vue'
import TestComponent from './components/TestComponent.vue';
import Table from './components/Table.vue';

export default {
  name: 'App',
  components: {
    // HelloWorld
    TestComponent,
    Table,
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 0;
}
</style>
