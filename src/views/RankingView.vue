<template lang="">
  <Navbar />
  <!-- <RankingCard /> -->
  <!-- <RankingCard title="Karla" @add-ranking="addRanking"/> -->

  <div class="ranking-page-wrapper">
    <div class=wrapper-buttons>
      <div></div>
      <Button class="discover-btn" text="All Steps" @click="switchOrderProp('avg_steps')" v-bind:class="[toggleOrder ? sortDirection = 'asc': sortDirection = 'desc' ]"></Button> 
      <Button class="discover-btn" text="Last Month" @click="switchOrderProp('avg_steps_month')" v-bind:class="[toggleOrderMonth ? sortDirection = 'asc': sortDirection = 'desc' ]"></Button> 
      <Button class="discover-btn"  text="Last Week" @click="switchOrderProp('avg_steps_week')" v-bind:class="[toggleOrderWeek ? sortDirection = 'asc': sortDirection = 'desc' ]"></Button> 
    </div>
    <div class="cards-flex-wrapper">
      <!-- 'sortedByAsc' is the computed property -->
      <div class="card-user-wrapper" v-for="ranking in sortByAsc" :key="ranking.id">
        <div class="username"><h1>{{ranking.username}}</h1></div>
        <div class="email"><p>e-mail: {{ranking.email}}</p></div>
        <div><img :src=getRandom() alt="profile-image"> </div> 
        <div class="ranking-text">
          <div >
            <p class="ranking-msg">Average steps</p>
            <p class="ranking-number">{{ranking.avg_steps}}</p>
          </div>
        
          <!-- <div class="ranking-text" v-else>
            <p >Ups! no steps last week.</p>
        </div> -->

        <router-link :to="{name: 'userId', params: { id: ranking.username }}" class="sort-wrapper-button"><Button class="details-btn" text="Discover"></Button></router-link>
      </div>
      </div> 
    </div>

    </div>
</template>


<script>
// @ is an alias to /src
import Navbar from "../components/Navbar.vue";
import RankingCard from "../components/RankingCard.vue";
import Button from "../components/Button.vue";
import girl from "../assets/girl.png";
import boy from "../assets/runner.png";
import boy1 from "../assets/boy-1.png";
import boy2 from "../assets/boy-2.png";
import girl1 from "../assets/girl-1.png";
import girl2 from "../assets/girl-2.png";
import axios from "axios";

const fetchPage = async (url) => {
  try {
    const res = await axios.get(url, {
      headers: {
        "Content-type": "application/json",
        Authorization: "Token af61aed7399dc6a636e443cdc8a2d55db97c524a",
      },
    });

    let results = res.data.results;

    if (res.data.next) {
      const next = new URL(res.data.next);
      next.protocol = "https";
      const page = await fetchPage(next.toString());
      results = [...results, ...page];
    }
    return results;
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
    // By last month
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
      }
      if (this.sortByKey === "avg_steps_month") {
        let sorted;
        if (this.toggleOrderMonth) {
          // Ascendent
          //console.log(this.sortByKey);
          sorted = this.rankingsByLastMonth.sort(
            (a, b) => b[this.sortByKey] - a[this.sortByKey]
          );
        } else {
          // Descendent
          //console.log(this.sortByKey);
          sorted = this.rankingsByLastMonth.sort(
            (a, b) => a[this.sortByKey] - b[this.sortByKey]
          );
        }
        return sorted;
      }
      if (this.sortByKey === "avg_steps_week") {
        let sorted;
        if (("by week - toggle order:", this.toggleOrderWeek)) {
          console.log("ASC!!!");
          // Ascendent
          console.log("by week - key:", this.sortByKey);
          sorted = this.rankingsByLastWeek.sort(
            (a, b) => b[this.sortByKey] - a[this.sortByKey]
          );
        } else {
          console.log("by week - DESC:", this.sortByKey);
          sorted = this.rankingsByLastWeek.sort(
            (a, b) => a[this.sortByKey] - b[this.sortByKey]
          );
        }
        return sorted;
      }
    },
  },
  methods: {
    // Get random images
    getRandom() {
      const images = [boy, girl, boy1, boy2, girl1, girl2];
      //console.log(images);
      return images[Math.floor(Math.random() * images.length)];
    },
    // ----- SORT METHODS --
    // with props to change field
    switchOrderProp(prop) {
      this.sortByKey = prop; //set the field to sort -> 'avg_steps'
      console.log("sortByKey", this.sortByKey);
      if (this.sortByKey === "avg_steps") {
        this.toggleOrder = !this.toggleOrder;
      }
      if (this.sortByKey === "avg_steps_week") {
        console.log("switch my week");
        this.toggleOrderWeek = !this.toggleOrderWeek;
      }
      if (this.sortByKey === "avg_steps_month") {
        console.log("switch my month");
        this.toggleOrderMonth = !this.toggleOrderMonth;
      }
    },

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
      //console.log(lastweek);
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
      //console.log("in show ranking");
      this.rankings = await fetchPage(
        "https://step-meter-pp4publmdq-ez.a.run.app/users/"
      );
    },

    async showRankingByLastDate(handler, rankingState, period) {
      //console.log("IMPORTANT FIELD", period);

      const periodStart = handler();
      const today = this.getToday();
      const endpoint = `https://step-meter-pp4publmdq-ez.a.run.app/users/?workouts_from=${periodStart}&workouts_to=${today}`;

      let data = await fetchPage(endpoint);

      const finalResults = data.map((userData) => {
        if (period === "month") userData.avg_steps_month = userData.avg_steps;
        if (period === "week") userData.avg_steps_week = userData.avg_steps;

        return userData;
      });
      //rankingState = finalResult;
      if (period === "month") this.rankingsByLastMonth = finalResults;
      if (period === "week") this.rankingsByLastWeek = finalResults;
    },
  },
};
</script>
<style scoped>
.cards-flex-wrapper {
  display: flex;
  flex-direction: column;
}
.card-user-wrapper {
  padding-top: 10rem;
  margin-top: 2rem;
  background-color: #fcfcfc;
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
  height: 200px;
  object-fit: contain;
  border-radius: 50%;
  background-color: #f8eddb;
  box-shadow: 0px 0px 4px rgb(79 78 78 / 15%);
}

.card-flex-wrappper > *:last-child {
  justify-self: flex-start;
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

.sort-wrapper-button {
  display: flex;
  justify-content: center;
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

@media (min-width: 600px) {
  .cards-flex-wrapper {
    flex-flow: row wrap;
    justify-content: center;
  }
  .card-user-wrapper {
    flex: 0 1 calc(50% - 4%);
    margin-right: 2%;
    margin-left: 2%;
  }
  .discover-btn {
    font-size: 1.3rem;
    padding: 15px 8px;
  }
}

@media (min-width: 768px) {
  .card-user-wrapper {
    flex: 0 1 calc(33.333333% - 4%);
    justify-content: flex-start;
  }
}
</style>