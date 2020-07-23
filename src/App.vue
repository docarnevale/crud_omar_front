<template>
  <div id="app">
   
    <PersonForm class="col-12"></PersonForm>
  </div>
</template>

<script>
//import HelloWorld from './components/HelloWorld.vue'
import PersonForm from './components/PersonForm.vue'

export default {
  name: 'App',
  components: {
    PersonForm
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
