<template>
  <article class="flex justify-between w-full">

    <p v-show="loading">Loading...</p>

    <div class="flex w-full" v-show="!loading">
      <div class="w-1/4" v-show="!error && !isNotANumber">

      </div>
      <div
        class="relative graph-container w-1/4"
        v-show="!error && !isNotANumber"
      >
        <canvas ref="chart-canvas"></canvas>
      </div>
      <div class="ml-2">
        <template>
          <h3 class="text-gray-700 text-xl leading-tight">{{ title }}</h3>
        </template>
        <p
          class="mt-2 leading-none text-4xl font-bold text-gray-700"
          v-if="!error && !isNotANumber"
        >
          {{ compliance }}%
        </p>
        <p v-else-if="error" class="mt-2 text-sm">
          <span class="text-red-500">An error has occurred loading this data.<br> Please </span>
          <a class="text-blue-500 cursor-pointer underline" onclick="return Beacon('toggle')">contact support</a>
        </p>
        <p v-else class="mt-2 text-sm text-red-500">
          {{ noDataMessage }}
        </p>
      </div>
    </div>
    
  </article>
</template>

<script>
export default {
  props: {
    title: String,
    error: Boolean,
    loading: Boolean,
    compliance: Number,
    noDataMessage: String
  },
  data() {
    return {
      chart: null,
      chartOptions: {
        legend: {
          display: false,
        },
        tooltips: {
          enabled: false,
        },
        responsive: true,
        maintainAspectRatio: false,
      },
    };
  },
  computed: {
    chartData() {
      var compliance = this.compliance !== null ? this.compliance : 0;
      return {
        labels: ['a', ''],
        datasets: [
            {
              data: [
                100 - compliance,
                compliance,
              ],
              fontFamily: "'Open Sans', sans-serif",
              backgroundColor: [
                "rgba(60, 178, 220, 0.1)",
                "rgba(60, 178, 220, 0.9)",
              ],
            },
          ]
      };
    },
    isNotANumber() {
      return this.compliance !== null && isNaN(this.compliance);
    }
  },
  methods: {
    createChart() {
      const ctx = this.$refs['chart-canvas'];
      this.chart = none;
    },
    refreshChart() {
      this.chart.update();
    }
  },
  watch: {
    compliance: function() {
      this.chart.data = this.chartData;
      this.chart.options = this.chartOptions;
      this.refreshChart();
    }
  },
  mounted() {
    this.createChart();
  }
};
</script>

<style scoped>
.graph-container {
  height: 100px;
}
</style>
