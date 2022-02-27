<template lang="">
    <div v-if="userData" >
      <apexchart width="500" type="heatmap" :options="chartOptions" :series="series"></apexchart>
         <p>{{userData}}</p>
         <p>{{period}}</p>
         <p>{{steps}}</p>
         <p>{{category}}</p>
       
    </div>
  <h1 class="load" v-else>On the way 🚀...</h1>
    
</template>

<script>
export default {
  name: "Heatmap",
  props: {
    userData: Array,
    period: Array,
    steps: Array,
    category: String,
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
      },
      series: [
        {
          //name: "Steps",
          data: this.steps,
          //data: this.steps,
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
      console.log(this.period);
      console.log("steps", this.steps);
      console.log(this.userData);
      console.log(this.category);
      this.series = [
        {
          name: this.category, // update the graph
          data: this.steps,
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
</style>