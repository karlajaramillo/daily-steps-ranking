<template lang="">


  <div class="wrapper-rankingCard">
    <img src="https://images.unsplash.com/photo-1506869640319-fe1a24fd76dc?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=2070&q=80">
    <div>
      <div>
      <!-- props -> {{title}} from <RankingCard title="Karla"/>--> 
      <h2>Username: {{title}}</h2>
    </div>
      <h2>Last week {{rankings}}</h2>
      <div>Graph 1</div>
    </div>
    <div>
      <h2>Last month</h2>
      <div>Graph 2</div>
    </div>
    <div>
      <h2>Overall daily average</h2>
      <div>Graph 3</div>
    </div>
	</div>
</template>
<script>
export default {
  name: "RankingCard",
  // define props
  props: {
    title: String, // type of prop
    rankings: Array,
  },
  components: {},
};
</script>
<style scoped>
img {
  box-shadow: 5px 5px 20px rgb(179 179 179 / 40%);
  border-radius: 4px;
}
</style>