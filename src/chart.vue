<template>
<canvas id="myChart" width="100" height="100" style="color:white"></canvas>

</template>

<script>
import { Line } from "vue-chartjs";
import Chart from "chart.js";
export default {
  props: ["data", "options"],
  mounted() {
    var ctx = document.getElementById("myChart").getContext("2d");
    var myChart = new Chart(ctx, {
      type: "doughnut",
      data: this.data,
      options: this.options
    });
  },
  methods: {
    renderChart() {
      var ctx = document.getElementById("myChart").getContext("2d");
      var myChart = new Chart(ctx, {
        type: "doughnut",
        data: this.data,
        options: this.options
      });
    }
  },
  watch: {
    data(val) {
      var ctx = document.getElementById("myChart").getContext("2d");
      var myChart = new Chart(ctx, {
        type: "doughnut",
        data: val,
        options: this.options
      });
    },
    options(val) {
      var ctx = document.getElementById("myChart").getContext("2d");
      var myChart = new Chart(ctx, {
        type: "doughnut",
        data: this.data,
        options: val
      });
    }
  }
};
</script>

<style scoped>
canvas {
  color: white !important;
}
</style>

