<template lang="">
  <Navbar />
  <RankingCard />
  <!-- <RankingCard title="Karla" @add-ranking="addRanking"/> -->

  <div class="ranking-page-wrapper">
   
    <Button class="discover-btn" @click="switchOrder()" v-bind:class="[toggleOrder ? sortDirection = 'asc': sortDirection = 'desc' ]" text="Daily Average"></Button> 

    <!-- <Button class="sortBySteps" @click="sortToggle('avg_steps')" v-bind:class="[sortByKey === 'avg_steps' ? sortDirection : '']" text="Test 1"></Button>  -->

    <div v-if="rankings.length">
       <!-- <Button class="sortBySteps" @click="sortToggle('avg_steps')" v-bind:class="[sortByKey === 'avg_steps' ? sortDirection : '']" text="Test 1"></Button> 
    <Button text="Average Steps Trial" @click="sort('avg_steps')" v-bind:class="[sortBy === 'avg_steps' ? sortDirection : '']"></Button>  -->

      <table class="table-section section-step" border="0" cellpadding="0" cellspacing="0">
        <thead>
            <tr class="table-h-wrapper">
              <th class="table-h">Name</th>
              <!-- <th class="table-h" @click="sortBy('avg_steps')" v-bind:class="[sortBy === 'avg_steps' ? sortDirection : '']">Average Steps</th> -->
              <th class="table-h" @click="switchOrderProp('avg_steps')" v-bind:class="[toggleOrder ? sortDirection = 'asc': sortDirection = 'desc' ]">Average Steps</th>
              <!-- <th class="table-h" @click="switchOrderProp('avg_steps_week')" v-bind:class="[toggleOrder ? sortDirection = 'asc': sortDirection = 'desc' ]">Last month</th> -->
              <th class="table-h" @click="switchOrderProp('avg_steps_month')" v-bind:class="[toggleOrder ? sortDirection = 'asc': sortDirection = 'desc' ]">Last week</th>
              <th class="table-h">Discover</th>
            </tr>
          </thead>
          <tbody>
            <!-- 'sortedArray' is the computed property -->
            <!-- <tr v-for="ranking in rankings" :key="ranking.id">    -->
            <tr v-for="ranking in sortByAsc" :key="ranking.id">        
              <td>{{ranking.username}}</td>
              <td>{{ranking.avg_steps}}</td>
              <td> <router-link :to="{name: 'userId', params: { id: ranking.username }}"><Button class="discover-btn" text="Discover"></Button></router-link>
              </td>
            </tr>
          </tbody>
      </table>
    </div>

  <div>
    <Button text="Sort by last week" @click="switchOrderProp('avg_steps_week')" v-bind:class="[toggleOrder ? sortDirection = 'asc': sortDirection = 'desc' ]"></Button> 

      <!-- 'sortedArray' is the computed property -->
      <!-- <tr v-for="ranking in rankings" :key="ranking.id">    -->
      <div v-for="rankingsByLastWeek in sortByAsc" :key="rankingsByLastWeek.id"> 
        <p>{{rankingsByLastWeek}}</p>       
        <p>{{rankingsByLastWeek.username}}</p>
        
        <div v-if="rankingsByLastWeek.avg_steps_week">
            <p>Steps last week: {{rankingsByLastWeek.avg_steps_week}}</p>
        </div>
          <div v-else>
      <p>Average Steps last week: Ups! no steps last week.</p>
      </div>

        <router-link :to="{name: 'userId', params: { id: rankingsByLastWeek.username }}"><Button class="discover-btn" text="Discover"></Button></router-link>
      </div>
  </div>


  <div v-if="rankingsByLastMonth.length" class="wrapper-cards">
    <!-- <Button text="Sort by last month" @click="switchOrderProp('avg_steps_month')" v-bind:class="[toggleOrder ? sortDirection = 'asc': sortDirection = 'desc' ]"></Button>  -->
    <Button text="Sort by last month" @click="switchOrderProp('avg_steps_month')" v-bind:class="[toggleOrder ? sortDirection = 'asc': sortDirection = 'desc' ]"></Button> 

      <!-- 'sortedArray' is the computed property -->
      <!-- <tr v-for="ranking in rankings" :key="ranking.id">    -->
      <div v-for="rankingsByLastMonth in sortByAsc" :key="rankingsByLastMonth.id">        
        <p>{{rankingsByLastMonth}}</p>
        <!-- <p>{{rankingsByLastMonth.avg_steps_month}}</p> -->
          <p>{{rankingsByLastMonth}}</p>
        <p>Steps by month: {{rankingsByLastMonth.avg_steps_month}}</p>
        
        <!-- <div v-if="rankingsByLastMonth.avg_steps_month">
            <p>Steps last month: {{rankingsByLastMonth.avg_steps_month}}</p>
        </div> 
               <div v-else>
      <p>Average Steps last month: Ups! no steps last month.</p>
      </div>
        
        -->
        <div v-if="rankingsByLastMonth.avg_steps">
             <p>Steps last month: {{rankingsByLastMonth.avg_steps}}</p>
        </div>
          <div v-else>
      <p>Average Steps last month: Ups! no steps last month.</p>
      </div>

        <router-link :to="{name: 'userId', params: { id: rankingsByLastMonth.username }}"><Button class="discover-btn" text="Discover"></Button></router-link>
      </div>
  </div>
    <!-- <div v-else>
      <p>Loading ranking...</p>
    </div> -->

    <!-- <div v-if="rankings.length" class="wrapper-cards">
      <Button class="discover-btn" @click="handleSortBy()" text="Sort by Steps"></Button>
      <div class="wrapper-rankingCard" v-for="ranking in rankings" title="Karla" :key="ranking.id">
        <div>
          <h2>Username: {{ranking.username}}</h2>
        </div>
        <div>
          <p>Average steps: {{ranking.avg_steps}} </p>
        </div>
        <div class="btn-wrapper">
          <router-link :to="{name: 'userId', params: { id: ranking.username }}"><Button class="discover-btn" text="Discover"></Button></router-link>
        </div>
      </div>
    </div>
    <div v-else>
      <p>Loading ranking...</p>
    </div> -->
  </div>


</template>


<script>
import Navbar from "../components/Navbar.vue";
import RankingCard from "../components/RankingCard.vue";
import Button from "../components/Button.vue";
const fetchPage = async (url) => {
  console.log("in fetchPage");
  console.log(url);
  try {
    const res = await fetch(url, {
      method: "GET",
      headers: {
        "Content-type": "application/json",
        // Authorization: "Token af61aed7399dc6a636e443cdc8a2d55db97c524a",
        Authorization: "Token af61aed7399dc6a636e443cdc8a2d55db97c524a",
      },
    });
    const data = await res.json();
    console.log(data);
    return data;
  } catch (err) {
    console.log(err);
  }
};
export default {
  name: "RankingView",
  components: {
    Navbar,
    RankingCard,
    Button,
  },
  data() {
    return {
      rankings: [],
      rankingsByLastMonth: [], // metric from last month to know
      rankingsByLastWeek: [], // metric from last day to know
      sortByKey: "avg_steps", //key to sort by default
      sortDirection: "asc", // DEFAULT - keep track of the sort order: ascending or descending
      toggleOrder: false,
      toggleOrderMonth: false,
    };
  },
  created: function () {
    this.showRankingByLastDate(
      this.getLastMonth,
      this.rankingsByLastMonth,
      "month"
    );
    // By last week
    this.showRankingByLastDate(
      this.getLastWeek,
      this.rankingsByLastWeek,
      "week"
    );
    this.showRanking();
    //this.showRankingByLastMonth();
    // By last month
  },
  // to make the value reactive
  computed: {
    // THIS METHOD WORK AND IS REACTIVE - CHANGES THE STATE - ASCENDING
    sortByAsc() {
      console.log(this.sortByKey);
      if (this.toggleOrder) {
        let sorted;
        if (this.sortByKey === "avg_steps") {
          console.log(this.sortByKey);
          sorted = this.rankings.sort(
            (a, b) => b[this.sortByKey] - a[this.sortByKey]
          );
        } else if (this.sortByKey === "avg_steps_month") {
          console.log(this.sortByKey);
          sorted = this.rankingsByLastMonth.sort(
            (a, b) => b[this.sortByKey] - a[this.sortByKey]
          );
        } else if (this.sortByKey === "avg_steps_week") {
          console.log(this.sortByKey);
          sorted = this.rankingsByLastWeek.sort(
            (a, b) => b[this.sortByKey] - a[this.sortByKey]
          );
        }
        return sorted;
      } else {
        // return this.rankings.sort(
        //   (a, b) => a[this.sortByKey] - b[this.sortByKey]
        // );
        let sortedDesc;
        if (this.sortByKey === "avg_steps") {
          console.log(this.sortByKey);
          sortedDesc = this.rankings.sort(
            (a, b) => a[this.sortByKey] - b[this.sortByKey]
          );
        } else if (this.sortByKey === "avg_steps_month") {
          console.log(this.sortByKey);
          sortedDesc = this.rankingsByLastMonth.sort(
            (a, b) => a[this.sortByKey] - b[this.sortByKey]
          );
        } else if (this.sortByKey === "avg_steps_week") {
          console.log(this.sortByKey);
          sortedDesc = this.rankingsByLastWeek.sort(
            (a, b) => a[this.sortByKey] - b[this.sortByKey]
          );
        }
        return sortedDesc;
      }
    },

    // sortByAscMonth() {
    //   if (this.toggleOrderMonth) {
    //     return this.rankingsByLastMonth.sort(
    //       (a, b) => b[this.sortByKey] - a[this.sortByKey]
    //     );
    //   } else {
    //     return this.rankingsByLastMonth.sort(
    //       (a, b) => a[this.sortByKey] - b[this.sortByKey]
    //     );
    //   }
    // },
    // sortByAscWeek() {
    //   if (this.toggleOrderWeek) {
    //     return this.rankingsByLastWeek.sort(
    //       (a, b) => b[this.sortByKey] - a[this.sortByKey]
    //     );
    //   } else {
    //     return this.rankingsByLastWeek.sort(
    //       (a, b) => a[this.sortByKey] - b[this.sortByKey]
    //     );
    //   }
    // },

    // THIS METHOD WORK AND IS REACTIVE - CHANGES THE STATE - ASCENDING

    // sortByAsc() {
    //   return this.rankings.sort((a, b) => {
    //     let modifier = 1;
    //     //if (this.sortDirection === "desc") modifier = -1;
    //     return a[this.sortByKey] < b[this.sortByKey]
    //       ? -1 * modifier
    //       : a[this.sortByKey] > a[this.sortByKey]
    //       ? 1 * modifier
    //       : 0;
    //   });
    // },

    // OPTION 1 - DON'T WORK
    // sortByAsc() {
    //   function compare(a, b) {
    //     if (a[this.sortByKey] < b[this.sortByKey]) return -1;
    //     if (a[this.sortByKey] > b[this.sortByKey]) return 1;
    //     return 0;
    //   }
    //   return this.rankings.sort(compare);
    // },
    // OPTION 2 - DON'T WORK
    // inside this computed property we call the method sort() an we pass inside the sorting logic
    // sortedRanking() {
    //   return this.rankings.sort((r1, r2) => {
    //     let modifier = 1;
    //     if (this.sortDirection === "desc") modifier = -1;
    //     if (r1[this.sortBy] < r2[this.sortBy]) return -1 * modifier;
    //     if (r1[this.sortBy] > r2[this.sortBy]) return 1 * modifier;
    //     return 0;
    //   });
    // },
  },
  methods: {
    // ----- SORT METHODS --
    // THIS METHOD WORKS WITH CLICK- BUT DON'T CHANGE THE STATE
    // sortBy(prop) {
    //   this.rankings.sort((a, b) => b[prop] - a[prop]);
    // },
    // // OPTION 1 - IT WORKS - to change the property
    // // to change the sortBy ->keep track of the property to sort, and sortDirection of the arrow
    // sortToggle(s) {
    //   if (s === this.sortByKey) {
    //     this.sortDirection = this.sortDirection === "asc" ? "desc" : "asc";
    //   }
    //   this.sortByKey = s;
    //   console.log("sortByKey", this.sortByKey);
    //   console.log("sortDirection", this.sortDirection);
    // },

    // IMPORTANT
    // with props to change field
    switchOrderProp(prop) {
      this.toggleOrder = !this.toggleOrder;
      this.sortByKey = prop; //set the field to sort -> 'avg_steps'
      console.log("toggle", this.toggleOrder);
      console.log("sortByKey", this.sortByKey);
    },

    switchOrderMonth() {
      this.toggleOrderMonth = !this.toggleOrderMonth;
      console.log("toggle month", this.toggleOrderMonth);
      console.log("sortByKey", this.sortByKey);
    },
    // switchOrder() {
    //   this.toggleOrder = !this.toggleOrder;
    //   console.log("toggle", this.toggleOrder);
    //   console.log("sortByKey", this.sortByKey);
    // },
    // ----- END - SORT METHODS --

    // ----- Method to get today in format YY-MM-DD --
    getToday() {
      const today = new Date();
      return today.toISOString().split("T").slice(0, 1).join();
    },
    // ----- Method to find last week data --
    getLastWeek() {
      const today = new Date();
      const lastweek = new Date(
        today.getFullYear(),
        today.getMonth(),
        today.getDate() - 7
      );
      console.log(lastweek);
      return lastweek.toISOString().split("T").slice(0, 1).join();
    }, // Result -> '2022-02-18'
    // ----- Method to find last month data --
    getLastMonth() {
      const today = new Date();
      const lastmonth = new Date(
        today.getFullYear(),
        today.getMonth() - 1,
        today.getDate()
      );
      console.log(lastmonth);
      return lastmonth.toISOString().split("T").slice(0, 1).join();
    }, // Result -> '2022-01-25'

    // ----- Fetch method to get daily average --
    async showRanking() {
      console.log("in show ranking");
      let data = await fetchPage(
        "https://step-meter-pp4publmdq-ez.a.run.app/users"
      );
      //console.log(`Next: ${data.next}`);
      // while (data.next) {
      //  data = await fetchPage(data.next);ftoken
      //   console.log(data.results);
      //   results.concat(data.results);
      // }
      console.log("after while");
      this.rankings = data.results;
      console.log(data.results);
    },

    async showRankingByLastDate(handler, rankingState, period) {
      console.log("IMPORTANT FIELD", period);

      const periodStart = handler();
      const today = this.getToday();
      const endpoint = `https://step-meter-pp4publmdq-ez.a.run.app/users?workouts_from=${periodStart}&workouts_to=${today}`;
      console.log("today: ", today);
      console.log("periodStart:", periodStart);
      let data = await fetchPage(endpoint);
      //console.log(`Next: ${data.next}`);
      // while (data.next) {
      //  data = await fetchPage(data.next);ftoken
      //   console.log(data.results);
      //   results.concat(data.results);
      // }
      const finalResults = data.results.map((userData) => {
        // if (period === "month") userData.avg_steps = userData.avg_steps_week;
        // if (period === "month") userData.avg_steps = userData.avg_steps_month;
        if (period === "month") userData.avg_steps_month = userData.avg_steps;
        if (period === "week") userData.avg_steps_week = userData.avg_steps;

        return userData;
      });

      // console.log(result);

      console.log("after while");
      console.log("results", [period, finalResults]);
      //rankingState = finalResult;
      if (period === "month") this.rankingsByLastMonth = finalResults;
      if (period === "week") this.rankingsByLastWeek = finalResults;
      console.log("this.rankingsByLastMonth", this.rankingsByLastMonth);
      console.log("this.rankingsByLastWeek", this.rankingsByLastWeek);
      //console.log("rankingState", rankingState);
      // console.log(period, data.results);
    },
  },
};
</script>
<style scoped>
table {
  /* border-collapse: collapse; */
  border-spacing: 0;
}
td {
  border-style: hidden !important;
}
tbody {
  text-align: center;
}
.table-section {
  border-top: 1em solid transparent;
  border: 0;
  display: flex;
  flex-direction: column;
}

.section-step {
  border-radius: 0.25em;
  border-collapse: separate;
  width: 100%;
  margin: 3rem auto;
  font-size: 1.2rem;
  width: 450px;
}
.section-step th {
  text-align: center;
  padding: 10px 20px;
}
.section-step td {
  font-size: 1.2rem;
  text-align: left;
  border-width: 3px 0;
  width: 50%;
  border-color: #efefef;
  background-color: #efefef;
  color: #333;
  padding: 40px 25px;
}
.section-step td:first-child {
  border-left-width: 3px;
  border-radius: 5px 0 0 5px;
}
.section-step td:last-child {
  border-right-width: 3px;
  border-radius: 0 5px 5px 0;
}
.section-step thead {
  display: table;
}
.section-step tbody {
  display: table;
  table-layout: fixed;
  border-spacing: 0 10px;
  flex-direction: column;
  display: flex;
}

td,
th {
  border: none;
}

.table-h-wrapper th:first-child {
  background-color: transparent;
  color: rgb(71, 71, 71);
}

.table-h-wrapper th:nth-child(2n) {
  padding: 10px 20px;
  background: #fe5f4f;
  color: #fff;
  font-family: "Sora";
  border-radius: 4px;
  text-transform: uppercase;
  font-weight: 300;
  font-size: 0.8rem;
}
.ranking-page-wrapper {
  padding-top: 4rem;
  padding-right: 3rem;
  padding-left: 3rem;
  display: flex;
  flex-direction: column;
}
.discover-btn {
  background-color: #fff;
  border-color: #333;
  color: #333;
  font-size: 0.8rem;
  padding: 10px 5px;
  border: 2px solid #333;
  margin-top: 0;
}

.discover-btn:hover {
  background: #474747;
  border-color: #474747;
  box-shadow: 0 0 8px rgb(0 0 0 / 26%);
  color: #fff;
}

.btn-wrapper {
  align-self: center;
}
.sortBySteps {
  background-color: #fff;
  border-color: #333;
  color: #333;
  width: 140px;
  font-size: 0.8rem;
  padding: 10px 5px;
  border: 2px solid #333;
}
.wrapper-cards {
  display: flex;
  flex-direction: column;
}
.wrapper-rankingCard {
  margin-top: 2rem;
  background-color: #e8e8e8;
  box-shadow: 0px 0px 1px rgb(34 34 34 / 30%);
  border-radius: 4px;
  padding: 1rem;
  display: flex;
  flex-direction: column;
}

.asc:after {
  content: "\25B2";
}

.desc:after {
  content: "\25BC";
}
</style>