<template lang="">
    <div v-if="userData">
      <apexchart id="apexchart" class="apexchart" type="heatmap" height="400" :options="chartOptions" :series="series"></apexchart>

  </div>   
  <h1 class="load" v-else>On the way 🚀...</h1>
    
</template>

<script>
export default {
  name: "HeatMap",
  props: {
    period: Array,
    steps: Array,
    calories: Array,
    distance: Array,
    active: Array,
    userData: Array,
  },

  data: function () {
    return {
      chartOptions: {
        dataLabels: {
          enabled: false,
          //enabled: true,
          // style: {
          //   colors: ["#888888"],
          //   fontSize: ".8rem",
          // },
        },
        title: {
          text: "All your activity",
          style: {
            fontSize: "20px",
            fontWeight: "600",
          },
        },
        chart: {
          fontFamily: "Quicksand",
        },
        // plotOptions: {
        //   heatmap: {
        //     colorScale: {
        //       ranges: [
        //         {
        //           from: 15000,
        //           to: 10000,
        //           color: "#32bfd1",
        //           name: "low",
        //         },
        //         {
        //           from: 10000,
        //           to: 1000,
        //           color: "#FFB200",
        //           name: "medium",
        //         },
        //         {
        //           from: 900,
        //           to: 300,
        //           color: "#F27036",
        //           name: "high",
        //         },
        //         {
        //           from: 300,
        //           to: 0,
        //           color: "#ff685c",
        //           name: "high",
        //         },
        //       ],
        //     },
        //   },
        // },
        colors: ["#32bfd1", "#F27036", "#ff685c", "#dbcf41"],

        xaxis: {
          type: "category",
          categories: this.period,
        },
      },
      series: [
        {
          name: "Steps",
          data: this.steps,
          // data: this.generateData(20, {
          //   min: 0,
          //   max: 15000,
          // }),
        },
        {
          name: "Calories",
          data: this.calories,
          // data: this.generateData(20, {
          //   min: 0,
          //   max: 900,
          // }),
        },
        {
          name: "Distance",
          data: this.distance,
          // data: this.generateData(20, {
          //   min: 0,
          //   max: 90,
          //}),
        },
        {
          name: "Active Minutes",
          data: this.active,
          //data: this.generateData(20, {
          // min: 0,
          // max: 90,
          //}),
        },
      ],
    };
  },
  created: function () {
    //this.updateChart();
  },

  methods: {
    generateData(count, yrange) {
      var i = 0;
      var series = [];
      while (i < count) {
        var x = (i + 1).toString();
        var y =
          Math.floor(Math.random() * (yrange.max - yrange.min + 1)) +
          yrange.min;
        series.push({
          x: x,
          y: y,
        });
        i++;
      }
      return series;
    },
    
    updateChart() {
      //pop up
      // console.log(this.period);
      // console.log("steps", this.steps);
      // console.log(this.userData);
      // console.log(this.category);
      // this.series = [
      //   {
      //     name: this.category, // update the graph
      //     data: this.steps,
      //   },
      // ];
    },
  },
};
</script>
<style scoped>
.load {
  padding-top: 6rem;
}
.apexcharts-canvas {
  width: 300px;
  margin: 0 auto;
}
.apexchart {
  height: 500px;
}

@media (min-width: 600px) {
  .apexcharts-canvas {
    width: 600px;
    margin: 0 auto;
  }
  .vue-apexcharts.apexchart {
    width: 600px;
  }
  .apexcharts-canvas {
    margin: 0 auto;
  }
  .vue-apexcharts .apexchart {
    height: 0;
  }
}
</style>