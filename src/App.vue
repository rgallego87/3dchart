<template>
  <div id="app" class="bkg">
    <chart3 class="center"/>
  </div>
</template>

<script>
// import chart from "./components/chart.vue";
// import chart2 from "./components/chart2.vue";
import chart3 from "./components/chart3.vue";

export default {
  name: "app",
  components: {
    chart3,
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #fff;
  margin-top: 60px;
}

.modebar {
  display: none !important;
}

.center {
  margin: 0 auto;
  width: 50%;
}

.bkg {
  background-color: rgb(53, 57, 67);
}
</style>
