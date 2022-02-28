<template lang="">
    <div v-if="userData" >
      <apexchart class="barChart" height="400" type="bar" :options="options" :series="series"></apexchart>       
         <!-- <p>{{userData}}</p>
         <p>{{period}}</p>
         <p>{{categoryArray}}</p>
         <p>{{category}}</p>
         <p>{{dataByWeek}}</p>
    
        -->
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
        chart: {
          id: "vuechart-example",
        },
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
      //pop up
      // console.log(this.dataByWeek); //undefined
      // console.log(this.period);
      // console.log("categoryArray", this.categoryArray);
      // console.log(this.userData);
      // console.log(this.category);
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
.apexcharts-canvas {
}
.barChart {
  /* width: 350px;
  height: 250px;
  overflow: scroll; */
}

@media (min-width: 600px) {
  .barChart {
    overflow: unset;
  }
}
</style>