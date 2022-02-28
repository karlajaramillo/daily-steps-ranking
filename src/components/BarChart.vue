<template lang="">
    <div v-if="userData" >
      <apexchart class="barChart" height="400" type="bar" :options="options" :series="series"></apexchart>       
    </div>
  <h1 class="load" v-else>On the way 🚀...</h1>
    
</template>

<script>
export default {
  name: "BarChart",
  props: {
    userData: Array,
    period: Array,
    categoryArray: Array,
    category: String,
    byWeek: Map,
    colorChart: String,
  },
  data: function () {
    return {
      options: {
        xaxis: {
          categories: this.period,
        },
        fill: {
          colors: [this.colorChart],
        },
        chart: {
          fontFamily: "Quicksand",
        },
        plotOptions: {
          bar: {
            borderRadius: 5,
          },
        },
        dataLabels: {
          enabled: true,
          style: {
            colors: ["#494947"],
            fontSize: ".8rem",
          },
        },
        title: {
          text: this.category,
          style: {
            fontSize: "20px",
            fontWeight: "600",
          },
        },
      },
      series: [
        {
          data: this.categoryArray,
        },
      ],
    };
  },
  created: function () {
    this.updateChart();
  },

  methods: {
    updateChart() {
      this.series = [
        {
          name: this.category, // update the graph
          data: this.categoryArray,
        },
      ];
    },
  },
};
</script>
<style scoped>
.load {
  padding-top: 6rem;
}

@media (min-width: 600px) {
  .barChart {
    overflow: unset;
  }
}
</style>