<template lang="">
    <div v-if="userData" >
      <apexchart width="500" type="bar" :options="options" :series="series"></apexchart>
        
         <p>{{userData}}</p>
         <p>{{period}}</p>
         <p>{{categoryArray}}</p>
         <p>{{category}}</p>
         <p>{{dataByWeek}}</p>
    
       
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
          data: this.categoryArray,
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
      console.log(this.dataByWeek); //undefined
      console.log(this.period);
      console.log("categoryArray", this.categoryArray);
      console.log(this.userData);
      console.log(this.category);
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
</style>