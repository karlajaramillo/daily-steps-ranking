<template lang="">
<div v-if="userData" class="wrapper-userCard">
	<img src="https://images.unsplash.com/photo-1476480862126-209bfaa8edc8?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxjb2xsZWN0aW9uLXBhZ2V8MnwxNDg3MTY2fHxlbnwwfHx8fA%3D%3D&auto=format&fit=crop&w=1200&q=60"> 
    <p>Username: {{ $route.params }}</p>
    <h1>Hello {{ username }}</h1>
    <p>User Id {{ userData }}</p>
  <div>
    <table border="">
      <thead>
        <tr>
          <th>Date</th>
          <th>Active minutes</th>
          <th>Calories</th>
          <th>Distance</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="user in userData" :key="user.id">
          <td>{{user.date}}</td>
          <td>{{user.active_minutes}}</td>
          <td>{{user.calories}}</td>
          <td>{{user.distance}}</td>
        </tr>
      </tbody>
    </table>
  </div>
  </div>
  <div v-else>
    <p class="loading">Loading ranking...</p>
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
      username: this.$route.params.id,
      //username: this.$route.params.id,
      userData: null,
    };
  },
  name: "UserIdCard",
  components: {},
  created: function () {
    this.showUser();
  },
  methods: {
    getData() {
      console.log(userData);
    },
    async showUser() {
      console.log("in show ranking");
      let data = await fetchPage(
        `https://step-meter-pp4publmdq-ez.a.run.app/${this.username}/workouts`
      );
      //console.log(`Next: ${data.next}`);
      // while (data.next) {
      //  data = await fetchPage(data.next);
      //   console.log(data.results);
      //   results.concat(data.results);
      // }
      console.log("after while");
      this.userData = data.results;
      console.log(this.userData);
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