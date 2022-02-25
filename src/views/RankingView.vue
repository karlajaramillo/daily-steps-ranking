<template lang="">
  <Navbar />
  <RankingCard />
  <!-- <RankingCard title="Karla" @add-ranking="addRanking"/> -->

  <div class="ranking-page-wrapper">
    <Button class="sortBySteps" @click="sortByHandle('avg_steps')" text="Sort by Steps"></Button>
    <Button class="sortBySteps" @click="sortUpdate('avg_steps')" text="Sort by Steps"></Button>

    <div>
      <table border="">
        <thead>
            <tr>
              <th>Name</th>
              <th @click="sortBy('avg_steps')" v-bind:class="[sortBy === 'avg_steps' ? sortDirection : '']">Average Steps</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="ranking in rankings" :key="ranking.id">
              <td>{{ranking.username}}</td>
              <td>{{ranking.avg_steps}}</td>
            </tr>
          </tbody>
      </table>
    </div>

    <div class="wrapper-cards">
      <Button class="discover-btn" @click="handleSortBy()" text="Sort by Steps"></Button>
    <div class="wrapper-rankingCard" v-for="ranking in rankings" title="Karla" :key="ranking.id">
      <div>
        <h2>Username: {{ranking.username}}</h2>
      </div>
      <div>
        <p>Average steps: {{ranking.avg_steps}} </p>
      </div>
      <div class="btn-wrapper">
        <!-- <router-link :to="{name: 'userId', params: { id: ranking.id }}"><Button class="discover-btn" text="Discover"></Button></router-link> -->
        <router-link :to="{name: 'userId', params: { id: ranking.username }}"><Button class="discover-btn" text="Discover"></Button></router-link>
      </div>
    </div>
    </div>
  </div>
<!-- 
  avg_active_minutes: 80
avg_calories: 317
avg_distance: 6465
avg_steps: 8484
date_joined: "2022-01-17T15:57:36.904429Z"
email: "xnoble@example.com"
id: 101 -->

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
      sortBy: "avg_steps", //key to sort by default
      sortDirection: "asc", // sort order
    };
  },
  created: function () {
    this.showRanking();
  },
  // to make the value reactive
  computed: {
    // sortedRaking() {
    //   return this.rankings.map((item) => item.avg_steps).sort((a, b) => b - a);
    // },
    sortedRanking: function () {
      return [...this.rankings].sort((a1, a2) => {
        let modifier = 1;
        if (this.sortDirection === "desc") modifier = -1;
        if (a1[this.sortBy] < a2[this.sortBy]) return -1 * modifier;
        if (a1[this.sortBy] > a2[this.sortBy]) return 1 * modifier;
        return 0;
      });
    },
  },
  methods: {
    onClick() {
      const result = this.rankings
        .map((item) => item.avg_steps)
        .sort((a, b) => b - a);
      console.log(result);
      return result;
    },
    handleSortBy() {
      rankings = sortByItem("avg_steps", this.rankings);
      console.log(this.rankings);
    },

    compareNumbers(a, b) {
      return b - a;
    },
    sortByItem(key, values) {
      const handler = compareNumbers;
      return [...values].sort((a, b) => {
        return handler(a[key], b[key]);
      });
    },

    async showRanking() {
      console.log("in show ranking");
      let data = await fetchPage(
        "https://step-meter-pp4publmdq-ez.a.run.app/users"
      );
      //console.log(`Next: ${data.next}`);
      // while (data.next) {
      //  data = await fetchPage(data.next);
      //   console.log(data.results);
      //   results.concat(data.results);
      // }
      console.log("after while");
      this.rankings = data.results;
      rankings: this.rankings;
      console.log(data.results);
    },
  },
  sort: function (s) {
    if (s === this.sortBy) {
      this.sortDirection = this.sortDirection === "asc" ? "desc" : "asc";
    }
    this.sortBy = s;
  },
  sortByHandle(prop) {
    console.log(prop);
    //this.rankings.sort((a, b) => (a[prop] < b[prop] ? -1 : 1));
  },
  sortUpdate(e) {
    console.log("hello");
  },
};
</script>
<style scoped>
.ranking-page-wrapper {
  padding-top: 4rem;
  padding-right: 3rem;
  padding-left: 3rem;
}
.discover-btn {
  background-color: #fff;
  border-color: #333;
  color: #333;
  width: 140px;
  font-size: 0.8rem;
  padding: 10px 5px;
  border: 2px solid #333;
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