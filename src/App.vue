<template>
  <div className="main">
    <UserComponent />
    <BeerComponent />
  </div>
</template>

<script>
import BeerComponent from "./components/Beer.vue";
import UserComponent from "./components/User.vue";

export default {
  name: "App",
  components: {
    BeerComponent,
    UserComponent,
  },
};
</script>

<style>
body {
  min-height: 100vh;
  display: block;
  margin: 0;
  background-image: url(@/assets/beer.jpeg);
  background-size: cover;
  background-position: center;
}

.main {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  align-items: flex-start;
  padding: 20px;
}
</style>
