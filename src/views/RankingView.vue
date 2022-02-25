<template lang="">
  <Navbar />
  <RankingCard title="Karla"/>
  <div class="">
    <!-- /user/:{{id}} -->
    <router-link to="/userId/1"><Button text="Discover"></Button></router-link>
  </div>
</template>


<script>
import Navbar from "../components/Navbar.vue";
import RankingCard from "../components/RankingCard.vue";
import Button from "../components/Button.vue";

export default {
  name: "RankingView",

  components: {
    Navbar,
    RankingCard,
    Button,
  },
};
</script>
<style scoped>
</style>


