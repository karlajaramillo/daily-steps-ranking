<template lang="">
  <div class="wrapper-rankingCard">
		<div>
      <!-- props -> {{username}} : Karla --> 
      <p>Username: Karla</p>
    </div>
		<img src="https://images.unsplash.com/photo-1476480862126-209bfaa8edc8?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxjb2xsZWN0aW9uLXBhZ2V8MnwxNDg3MTY2fHxlbnwwfHx8fA%3D%3D&auto=format&fit=crop&w=1200&q=60"> 
		<div>
      <h2>Last week</h2>
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
  components: {},
};
</script>
<style lang="">
</style>