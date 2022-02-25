<template lang="">
<div class="wrapper-userCard">
    <div>
      <p>Username: Karla</p>
      
    </div>
		<img src="https://images.unsplash.com/photo-1476480862126-209bfaa8edc8?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxjb2xsZWN0aW9uLXBhZ2V8MnwxNDg3MTY2fHxlbnwwfHx8fA%3D%3D&auto=format&fit=crop&w=1200&q=60"> 
     <p>Username: Karla {{ $route.params }}</p>
     <p>User Id {{ id }}</p>
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
  data() {
    return {
      id: this.$route.params.id,
      //username: this.$route.params.id,
      userRankings: [],
    };
  },
  name: "UserIdCard",
  components: {},
  created: function () {
    this.showUser();
  },
  methods: {
    async showUser() {
      console.log("in show ranking");
      let data = await fetchPage(
        "https://step-meter-pp4publmdq-ez.a.run.app/hrodriguez/workouts"
      );
      //console.log(`Next: ${data.next}`);
      // while (data.next) {
      //  data = await fetchPage(data.next);
      //   console.log(data.results);
      //   results.concat(data.results);
      // }
      console.log("after while");
      this.userRankings = data.results;
      userRankings: this.userRankings;
      console.log(data.results);
    },
  },
};
</script>
<style scoped>
img {
  box-shadow: 5px 5px 20px rgb(179 179 179 / 40%);
  border-radius: 4px;
}
</style>