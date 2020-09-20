<template>
  <div id="app">
    <ChartComponent
        class="chart-opt"
        :options="chartOptions"
        :chartData="chartData"
    />
    <label>
      <input v-model.number="startRange" type="number">
    </label>
    <label>
      <input v-model.number="endRange" type="number">
    </label>
    <label>
      <input v-model.number="rangeStep" :min="0.1" type="number" :step="0.1">
    </label>
  </div>
</template>

<script>
import ChartComponent from "@/components/ChartComponent.js";

export default {
  name: 'App',
  components: {
    ChartComponent
  },

  data() {
    return {
      startRange: 1,
      endRange: 5,
      rangeStep: 1,
      chartOptions: {
        responsive: true,
        maintainAspectRatio: false,
        scales: {
          yAxes: [{
            ticks: {
              beginAtZero: true,
              stepSize: 1
            }
          }]
        }
      }
    }
  },

  computed: {
    labels() {
      let list = [];
      if (this.startRange > this.endRange || this.rangeStep <= 0) {
        return []
      }
      for (let i = this.startRange; i <= this.endRange; i += this.rangeStep) {
        list.push(i);
      }
      return list;
    },

    chartData() {
      return {
        labels: this.labels,
        datasets: [{
          label: "f(x)",
          borderColor: "rgba(75, 192, 192, 1)",
          data: this.labels.map(x => this.calcFunc(x)),
          fill: false
        }
        ]
      }
    },

    chartStyle() {
      return {
        height: "400px",
        position: 'relative'
      }
    }
  },

  methods: {
    calcFunc(x) {
      return (0.1 + 0.3 * x * x * x) / (5 + Math.sqrt(0.15 + x * x * x * x))
    },
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
  height: 400px;
//margin-top: 60px;
}

.chart-container {
  position: relative;
  margin: auto;
  height: 80vh;
  width: 80vw;
}

.chart-opt {
  height: 80vh;
}
</style>
