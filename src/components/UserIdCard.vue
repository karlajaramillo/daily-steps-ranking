<template lang="">
<div v-if="userData" class="">
  <div class="wrapper-user-card">
    <div class="image-wrapper"><img :src="image"></div> 
    <h1>Uncover your data, and go the the next level</h1>
    <p><span class="msg">"</span>Check statistics and keep your productivity up<span class="msg">" </span> <span class="author"> by Mary Bye</span></p>
  </div>
  <h1 class="welcome-user">Your Last Report <span class="username">@{{ username }}</span></h1>
  <div class="barCharts-wrapper">
    <BarChart class="barChart" :colorChart="colorSteps" :category="categorySteps" :dataByWeek="dataByWeek" :userData="userData" :period="getPeriod" :categoryArray="getSteps"/>
    <BarChart class="barChart" :colorChart="colorCalories" :category="categoryCalories" :dataByWeek="dataByWeek" :userData="userData" :period="getPeriod" :categoryArray="getCalories"/>
    <BarChart class="barChart" :colorChart="colorDistance" :category="categoryDistance" :dataByWeek="dataByWeek" :userData="userData" :period="getPeriod" :categoryArray="getDistance"/>
    <BarChart class="barChart" :colorChart="colorActive" :category="categoryActive" :dataByWeek="dataByWeek" :userData="userData" :period="getPeriod" :categoryArray="getActiveMinutes"/>
   </div>
  <div class="heatmap-container">
    <h1 class="welcome-user">Track your activity at a glance</h1>
    <div class="heatmap-wrapper">
      <HeatMap class="heatmapChart" :userData="userData" :period="getPeriod" :steps="getSteps" :calories="getCalories" :distance="getDistance" :active="getActiveMinutes"></HeatMap>
    </div>
  </div>

  </div>
  <div v-else>
    <p class="loading">Loading ranking...</p>
  </div>

</template>
<script>
// import components
import HeatMap from "./HeatMapGraph.vue";
import BarChart from "../components/BarChart.vue";
import image from "../assets/photo-users.png";
import axios from "axios";

const fetchPage = async (url) => {
  try {
    const res = await axios.get(url, {
      headers: {
        "Content-type": "application/json",
        Authorization: "Token af61aed7399dc6a636e443cdc8a2d55db97c524a",
      },
    });

    return res.data;
  } catch (err) {
    console.log(err);
  }
};

export default {
  data() {
    return {
      username: this.$route.params.id,
      userData: null,
      categorySteps: "Steps",
      categoryCalories: "Calories",
      categoryDistance: "Distance",
      categoryActive: "Active minutes",
      colorSteps: "#ff685c",
      colorCalories: "#ffd06a",
      colorDistance: "#32bfd1",
      colorActive: "#dbcf41",
      dataByWeek: null,
      image: image, // bind image to my component
    };
  },
  name: "UserIdCard",
  components: {
    //register all the components imported
    BarChart,
    HeatMap,
  },
  created: function () {
    this.showUser();
  },
  computed: {
    getPeriod() {
      console.log("period Series");
      console.log(this.userData.map((item) => item.date));
      return this.userData.map((item) => item.date);
    },
    getSteps() {
      console.log(
        "steps",
        this.userData.map((item) => item.steps)
      );
      return this.userData.map((item) => item.steps);
    },
    getCalories() {
      return this.userData.map((item) => item.calories);
    },
    getDistance() {
      return this.userData.map((item) => Math.floor(item.distance));
    },
    getActiveMinutes() {
      //active_minutes
      return this.userData.map((item) => item.active_minutes);
    },
    getByWeek() {
      const data = new Map();
      const keys = [];
      const emptyData = {
        steps: 0,
        distance: 0,
        calories: 0,
        active_minutes: 0,
      };

      for (let i = 1; i <= 52; i++) {
        keys.push(`2021-W${i}`);
      }

      for (let i = 1; i <= 12; i++) {
        keys.push(`2022-W${i}`);
      }

      for (let key of keys) {
        data.set(key, emptyData);
      }

      this.userData.forEach((workout) => {
        const date = new Date(workout.date);
        const key = `${date.getFullYear()}-W${this.weekOfYear(workout.date)}`;
        const currentData = data.get(key);
        const updatedData = {
          steps: currentData.steps + workout.steps,
          distance: currentData.distance + workout.distance,
          calories: currentData.calories + workout.calories,
          active_minutes: currentData.active_minutes + workout.active_minutes,
        };
        data.set(key, updatedData);
      });
      return data;
    },
  },
  methods: {
    // Get today in the format -> 2022-28-02
    getToday() {
      const today = new Date();
      return today.toISOString().split("T").slice(0, 1).join();
    },
    weekOfYear() {
      let currentdate = new Date("2022-01-05");
      // first day of the current Year
      // getFullYear -> 0: first month 1: first day of the year
      let firstDayYear = new Date(currentdate.getFullYear(), 0, 1);
      let numberOfDays = Math.floor(
        (currentdate - firstDayYear) / (24 * 60 * 60 * 1000)
      );
      // get Day + 1, because it starts at day 0
      return Math.ceil((currentdate.getDay() + 1 + numberOfDays) / 7);
      //console.log(
      //  `The week number of the current date (${currentdate}) is ${result}.`
      //);
    },
    // Fetch from API data by user
    async showUser() {
      console.log("in show ranking");
      let data = await fetchPage(
        `https://step-meter-pp4publmdq-ez.a.run.app/${this.username}/workouts/`
      );

      this.userData = data.results;
    },
  },
};
</script>
<style scoped>
.username {
  text-transform: capitalize;
  margin-bottom: 1rem;
}
.wrapper-user-card {
  background-color: #98dfea;
  /* background: linear-gradient(
    0deg,
    rgba(55, 213, 206, 1) 0%,
    rgba(72, 152, 213, 1) 100%
  ); */
  width: 100%;
  padding: 2rem 2.5rem;
  display: flex;
  flex-direction: column;
  align-items: center;
  border-radius: 4px;
  box-shadow: 1px 1px 3px rgb(34 34 34 / 15%);
}

.wrapper-user-card h1 {
  margin-top: 1rem;
  font-size: 1.5rem;
  font-family: "Merriweather", serif;
  font-weight: 700;
}

.wrapper-user-card p {
  margin-top: 1rem;
  font-size: 1.1rem;
  font-family: "Merriweather", serif;
}

.wrapper-user-card .msg {
  font-size: 1rem;
  font-family: "Lobster", sans-serif;
}
.wrapper-user-card .author {
  font-size: 0.8rem;
  font-weight: 100;
}

.image-wrapper {
  border-bottom: 4px solid #2dc3d363;
  border-radius: 4px;
  max-width: 600px;
}
.image-wrapper {
  margin-top: 20%;
}
.barChart {
  background-color: #fcfcfc;
  margin: 1rem;
  padding: 2rem;
  border-radius: 8px;
}

.heatmapChart {
  background-color: #fcfcfc;
  margin: 1rem;
  padding: 4rem;
  border-radius: 8px;
  width: 100%;
}
.welcome-user {
  margin-top: 2rem;
  text-align: center;
  color: #494947;
}

.heatmap-wrapper {
  display: flex;
  justify-content: center;
  margin-top: 1rem;
  border-radius: 8px;
}

@media (min-width: 600px) {
  .wrapper-user-card {
    padding: 6rem 6rem;
  }
  .image-wrapper {
    margin-top: 4rem;
  }
  .wrapper-user-card h1 {
    font-size: 2.5rem;
  }
  .wrapper-user-card p {
    margin-top: 1rem;
    font-size: 1.5rem;
  }
  .wrapper-user-card p {
    font-size: 1.5rem;
  }

  .wrapper-user-card .msg {
    font-size: 2rem;
  }
  .wrapper-user-card .author {
    font-size: 1rem;
  }
  .barCharts-wrapper {
    display: flex;
    flex-flow: row wrap;
    justify-content: center;
    margin: 3rem;
  }
  .barCharts-wrapper .barChart {
    width: 33%;
  }
  .heatmapChart {
    display: flex;
    justify-content: center;
    margin: 3rem;
    width: 100%;
  }
}
</style>