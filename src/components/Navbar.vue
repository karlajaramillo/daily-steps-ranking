<template lang="en">
 <nav class="navbar">
   <div class="wrapper-logo">
    </div>
    <router-link to="/"><p class="logo">activity<span>Meter</span></p></router-link>
  </nav>
</template>

<script>
export default {
  name: "Navbar",
  components: {},
};
</script>

<style scoped>
.wrapper-logo {
  display: flex;
}
.navbar {
  display: flex;
  align-items: center;
  justify-content: flex-start;
  background-color: #efe02b;
  position: fixed;
  min-width: 100%;
  box-shadow: 2px 2px 10px rgb(34 34 34 / 30%);
  border-bottom-left-radius: 4px;
  border-bottom-right-radius: 4px;
  z-index: 5000;
  height: 3.5rem;
}

.navbar a {
  color: #fff;
}

.logo {
  font-family: "Quicksand";
  margin-left: 1.5rem;
  color: #fff;
  font-weight: 600;
  font-size: 1.5rem;
  text-shadow: 0px 0px 1px rgb(160, 159, 159);
}

.logo span {
  color: #ff5f4f;
  font-family: "Lobster";
  margin-left: 0.1rem;
  font-size: 1.5rem;
  text-shadow: 0px 0px 1px rgb(160, 159, 159);
}
</style>