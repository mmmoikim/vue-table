<template>
  <div id="app">
    <Table />
  </div>
</template>

<script>
import Table from './components/VueTable/VueTable.vue'

export default {
  name: 'app',
  components: {
    Table
  },
  data(){
    return{
    }
  },
  methods:{
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
