<template lang="">
  <Navbar />
  <RankingCard/>
  <div class="">
    <!-- /user/:{{id}} -->
    <router-link to="/userId/1">Uncover your track!</router-link>
  </div>
</template>


<script>
import Navbar from "../components/Navbar.vue";
import RankingCard from "../components/RankingCard.vue";

export default {
  name: "RankingView",

  components: {
    Navbar,
    RankingCard,
  },
};
</script>
<style lang="">
</style>


