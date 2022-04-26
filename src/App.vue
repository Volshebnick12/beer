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
  display: block;
  padding: 20px;
  margin: 0;
  background-image: url(@/components/image/beer.jpg);
  background-size: cover;
  background-position: center;
}

.app {
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Oxygen,
    Ubuntu, Cantarell, "Open Sans", "Helvetica Neue", sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

.main {
  display: flex;
  justify-content: center;
  align-items: center;
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
