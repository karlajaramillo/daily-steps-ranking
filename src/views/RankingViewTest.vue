<template lang="">
  <Navbar />
  <RankingCard />
  <!-- <RankingCard title="Karla" @add-ranking="addRanking"/> -->

  <div class="ranking-page-wrapper">
    <div class=wrapper-buttons>
      <Button class="discover-btn" text="All Steps" @click="switchOrderProp('avg_steps')" v-bind:class="[toggleOrder ? sortDirection = 'asc': sortDirection = 'desc' ]"></Button> 
      <Button class="discover-btn" text="Last Month" @click="switchOrderProp('avg_steps_month')" v-bind:class="[toggleOrderMonth ? sortDirection = 'asc': sortDirection = 'desc' ]"></Button> 
      <Button class="discover-btn"  text="Last Week" @click="switchOrderProp('avg_steps_week')" v-bind:class="[toggleOrderWeek ? sortDirection = 'asc': sortDirection = 'desc' ]"></Button> 
    </div>
    
      <!-- 'sortedArray' is the computed property -->
      <!-- <tr v-for="ranking in rankings" :key="ranking.id">    -->
      <div class="card-user-wrapper" v-for="ranking in sortByAsc" :key="ranking.id"> 
        <div class="username"><h1>{{ranking.username}}</h1></div>
        <div class="email"><p>e-mail: {{ranking.email}}</p></div>
        <div><img src="https://cdn-icons-png.flaticon.com/512/2553/2553967.png" alt="profile-image"> </div> 
        <div class="ranking-text">
          <div >
            <p class="ranking-msg">Average steps</p>
            <p class="ranking-number">{{ranking.avg_steps}}</p>
          </div>
        </div>
          <!-- <div class="ranking-text" v-else>
            <p >Ups! no steps last week.</p>
        </div> -->

        <router-link :to="{name: 'userId', params: { id: ranking.username }}"><Button class="details-btn" text="Discover"></Button></router-link>
      </div>
    </div>
  </template>


<script>
  // @ is an alias to /src
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
      toggleOrderWeek: false,
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
    sortByAsc() {
      console.log(this.sortByKey);
      if (this.sortByKey === "avg_steps") {
        let sorted;
        if (this.toggleOrder) {
          // Ascendent
          console.log(this.sortByKey);
          sorted = this.rankings.sort(
            (a, b) => b[this.sortByKey] - a[this.sortByKey]
          );
        } else {
          // Descendent
          console.log(this.sortByKey);
          sorted = this.rankings.sort(
            (a, b) => a[this.sortByKey] - b[this.sortByKey]
          );
        }
        return sorted;
      } else if (this.sortByKey === "avg_steps_month") {
        let sorted;
        if (this.toggleOrderMonth) {
          // Ascendent
          console.log(this.sortByKey);
          sorted = this.rankingsByLastMonth.sort(
            (a, b) => b[this.sortByKey] - a[this.sortByKey]
          );
        } else {
          // Descendent
          console.log(this.sortByKey);
          sorted = this.rankingsByLastMonth.sort(
            (a, b) => a[this.sortByKey] - b[this.sortByKey]
          );
        }
        return sorted;
      } else if (this.sortByKey === "avg_steps_week") {
        let sorted;
        if (this.toggleOrderWeek) {
          // Ascendent
          console.log(this.sortByKey);
          sorted = this.rankingsByLastWeek.sort(
            (a, b) => b[this.sortByKey] - a[this.sortByKey]
          );
        } else {
          console.log(this.sortByKey);
          sorted = this.rankingsByLastWeek.sort(
            (a, b) => a[this.sortByKey] - b[this.sortByKey]
          );
        }
        return sorted;
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
      this.sortByKey = prop; //set the field to sort -> 'avg_steps'
      console.log("sortByKey", this.sortByKey);
      if (this.sortByKey === "avg_steps") {
        this.toggleOrder = !this.toggleOrder;
      } else if (this.sortByKey === "avg_steps_weeks") {
        this.toggleOrderWeek = !this.toggleOrderWeek;
      } else if (this.sortByKey === "avg_steps_month") {
        this.toggleOrderMonth = !this.toggleOrderMonth;
      }
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
      // console.log("this.rankingsByLastMonth", this.rankingsByLastMonth);
      // console.log("this.rankingsByLastWeek", this.rankingsByLastWeek);
      //console.log("rankingState", rankingState);
      // console.log(period, data.results);
    },
  },
};
</script>
<style scoped>
.card-user-wrapper {
  padding-top: 10rem;
  margin-top: 2rem;
  background-color: #f1f1f1;
  box-shadow: 1px 1px 3px rgb(34 34 34 / 15%);
  border-radius: 4px;
  padding: 1rem;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
.card-user-wrapper img {
  width: 200px;
  border-radius: 50%;
  background-color: #fffdfd;
  box-shadow: 0px 0px 4px rgb(34 34 34 / 15%);
}

.username {
  text-transform: capitalize;
  margin-bottom: 1rem;
}

.email p {
  color: #8d9190;
  font-size: 1.2rem;
  margin-bottom: 1rem;
}

.ranking-text .ranking-msg {
  font-size: 1.25rem;
  font-weight: 600;
  text-transform: uppercase;
  text-align: center;
  margin: 1rem 0;
}

.ranking-text .ranking-number {
  color: #2ac48a;
  text-shadow: 0px 0px 1px #656464;
  font-size: 2rem;
  /* font-weight: 600; */
  text-align: center;
  margin: 1rem 0;
  font-family: "Oswald";
}

.details-btn {
  background-color: #fdb12a;
  border-color: #fdb12a;
  color: #fff;
  font-size: 0.8rem;
  padding: 7px 10px;
  border: 2px solid #fdb12a;
  margin-top: 0;
  border-radius: 8px;
  font-weight: 500;
}

.details-btn:hover {
  background: #e6b259;
  border-color: #e6b259;
  box-shadow: 0 0 8px rgb(0 0 0 / 26%);
  color: #fff;
}

.wrapper-buttons {
  display: flex;
  justify-content: space-evenly;
}

.discover-btn {
  background-color: #fff;
  border-color: #333;
  color: #333;
  font-size: 0.7rem;
  padding: 10px 3px;
  border: 2px solid #333;
  margin-top: 0;
  display: flex;
  justify-content: space-evenly;
  align-items: center;
  margin: 6px;
}

.discover-btn:hover {
  background: #474747;
  border-color: #474747;
  box-shadow: 0 0 8px rgb(0 0 0 / 26%);
  color: #fff;
}

.asc:after {
  content: "\25B2";
  margin-right: 4px;
}

.desc:after {
  content: "\25BC";
  margin-right: 4px;
}

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
  padding-top: 4rem;
  padding-right: 1rem;
  padding-left: 1rem;
  display: flex;
  flex-direction: column;
  margin: 0 auto;
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
</style>