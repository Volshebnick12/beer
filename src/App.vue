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
  justify-content: center;
  align-items: center;
  padding: 20px;
}

@media screen and (max-width: 900px) {
  body {
    height: 100vh;
  }

  .main {
    display: block;
  }
}
</style>
