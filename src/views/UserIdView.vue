<template lang="">
    <div>
        <Navbar/>
        <UserIdCard/>
        
    </div>
</template>
<script>
import Navbar from "../components/Navbar.vue";
import UserIdCard from "../components/UserIdCard.vue";
export default {
  name: "UserIdView",

  components: {
    Navbar,
    UserIdCard,
  },
};
</script>
<style lang="">
</style>