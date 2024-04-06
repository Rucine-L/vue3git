<template>
  <div id="app">
    <d3-pie-chart></d3-pie-chart>
  </div>
</template>

<script>
import D3PieChart from './components/D3PieChart.vue';

export default {
  components: {
    D3PieChart
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
