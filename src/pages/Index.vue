<template>
  <q-page class="flex flex-center">
    <div class="q-pa-md">
      <q-badge color="secondary">
        Model: {{ value }}. Move the slider to see the chart update.
      </q-badge>

      <q-slider
        v-model="value"
        :min="0"
        :max="50"
        :inner-min="10"
        :inner-max="35"
        @update:model-value="updateChart"
      />
      <canvas id="reactive-chart"></canvas>
    </div>
  </q-page>
</template>

<script>
import { ref } from "vue";
import { Chart, registerables } from "chart.js";

export default {
  name: "PageIndex",
  setup() {
    var reactiveChart = undefined;
    return {
      value: ref(25),
      chartData: [12, 19, 3, 5, 2, 3],
    };
  },
  mounted() {
    Chart.register(...registerables);

    let ctx = document.getElementById("reactive-chart");

    this.reactiveChart = new Chart(ctx, {
      type: "line",
      data: {
        labels: ["1", "2", "3", "4", "5", "6"],
        datasets: [
          {
            label: "Values",
            data: this.chartData,
            backgroundColor: [],
            borderColor: [],
            borderWidth: 2,
          },
        ],
      },
      options: {
        scales: {
          y: {
            beginAtZero: true,
          },
        },
      },
    });
  },
  methods: {
    updateChart(value) {
      for (var i = 0; i < this.chartData.length; ++i) {
        this.chartData[i] = (value % (i + 1)) * 10 + (i - 10);
      }
      this.reactiveChart.data.datasets.forEach((dataset) => {
        dataset.data = this.chartData;
      });
      console.log(this.reactiveChart)
      this.reactiveChart.update();
    },
  },
};
</script>
