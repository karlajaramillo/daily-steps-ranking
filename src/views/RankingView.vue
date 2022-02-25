<template lang="">
  <Navbar />
  <RankingCard title="Karla" :rankings="rankings"/>
  <!-- <RankingCard title="Karla" @add-ranking="addRanking"/> -->
  <div class="">
    <!-- /user/:{{id}} -->
    <router-link to="/userId/1"><Button text="Discover"></Button></router-link>
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
    };
  },
  created: function () {
    this.showRanking();
  },
  methods: {
    async showRanking() {
      console.log("in show ranking");
      let data = await fetchPage(
        "https://step-meter-pp4publmdq-ez.a.run.app/users"
      );

      console.log(`Next: ${data.next}`);
      // while (data.next) {
      //  data = await fetchPage(data.next);
      //   console.log(data.results);
      //   results.concat(data.results);
      // }

      console.log("after while");

      this.rankings = [...this.rankings, data];
      rankings: this.rankings;
      console.log(rankings);
      console.log(data);
    },
  },
};
</script>
<style scoped>
</style>


