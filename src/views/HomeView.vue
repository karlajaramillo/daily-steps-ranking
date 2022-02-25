<template>
  <Navbar />
  <div class="home">
    <h1>Daily Steps Ranking</h1>
    <!-- <div class="msg-wrapper">
      <p>
        Discover how trackMe help you go to the next level. Track your daily
        steps. Still curious about how it works?
      </p>
    </div> -->

    <div class="wrapper-icon">
      <font-awesome-icon :icon="myIcon" class="icon-arrow fa-fade" />
    </div>

    <div class="ranking-page">
      <router-link to="/ranking"
        ><Button text="Find out more!"></Button
      ></router-link>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import { FontAwesomeIcon } from "@fortawesome/vue-fontawesome";
import { faAngleDown } from "@fortawesome/free-solid-svg-icons";
import Navbar from "../components/Navbar.vue";
import Button from "../components/Button";

export default {
  name: "HomeView",
  data() {
    return {
      myIcon: faAngleDown,
    };
  },

  components: {
    FontAwesomeIcon,
    Navbar,
    Button,
  },
};
</script>


<style>
.home {
  background-image: url("https://images.unsplash.com/photo-1465188162913-8fb5709d6d57?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1770&q=80");
  height: 100vh;
  width: 100%;
  background-repeat: no-repeat;
  background-position: center;
  background-size: cover;
  display: flex;
  justify-content: space-evenly;
  align-items: center;
  flex-direction: column;
  text-align: center;
  background-attachment: fixed;
  box-shadow: 0 0 8px rgba(0, 0, 0, 0.26);
}
.home h1 {
  color: #fff;
  font-family: "Lobster", cursive;
  font-size: 3rem;
  text-shadow: 1px 1px 10px #000;
}
.wrapper-icon {
  /* position: absolute;
  bottom: 10%;
  left: 50%;
  transform: translate(-50%, -50%); */
}
.icon-arrow {
  position: absolute;
  bottom: 25%;
  left: 50%;
  transform: translate(-50%, -50%);
  font-size: 4rem;
  color: #fff;
  --fa-animation-duration: 3s;
  --fa-animation-iteration-count: 1;
}
.msg-wrapper {
  background-color: #fdfdfd54;
  padding: 0.5rem 2rem;
  margin: 2rem 2rem 1rem 2rem;
  border-radius: 8px;
}

.msg-wrapper p {
  color: #333;
  font-size: 1.1rem;
  font-weight: 500;
}
</style>