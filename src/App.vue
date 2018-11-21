<template>
  <div id="app">
    <chart />  
  </div>
</template>

<script>
import chart from "./components/chart.vue";


export default {
  name: "app",
  components: {    
    chart
  }  
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.modebar {
  display: none !important;
}

.center {
  margin: 0 auto;
  width: 60%;
}
</style>
