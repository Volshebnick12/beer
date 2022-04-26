<template>
  <div class="beer">
    <div className="container" v-if="!loading && data">
      <div className="text__block">
        <h4>Бренд:</h4>
        {{ data.brand }}
      </div>
      <div className="text__block">
        <h4>Название:</h4>
        {{ data.name }}
      </div>
      <div className="text__block">
        <h4>Стиль:</h4>
        {{ data.style }}
      </div>
      <div className="text__block">
        <h4>Хмель:</h4>
        {{ data.hop }}
      </div>
      <div className="text__block">
        <h4>Солод:</h4>
        {{ data.malts }}
      </div>
      <div className="text__block">
        <h4>Алк:</h4>
        {{ data.alcohol }}
      </div>
      <div>
        <h4 className="text__block">IBU:</h4>
        {{ data.ibu }}
      </div>
      <div className="text__block">
        <h4>Плотность:</h4>
        {{ data.blg }}
      </div>
      <button className="button" @click="fetchData">Хочу другое пиво!</button>
    </div>
    <p v-if="loading">Loading...</p>
  </div>
</template>

<script>
import { ref, onMounted } from "vue";

export default {
  name: "BeerComponent",
  props: {},
  setup() {
    const data = ref(null);
    const loading = ref(true);
    const error = ref(null);

    const fetchData = () => {
      console.log("fetchData call");
      loading.value = true;
      return fetch("https://random-data-api.com/api/beer/random_beer", {
        method: "get",
        headers: {
          "content-type": "application/json",
        },
      })
        .then((res) => {
          // a non-200 response code
          if (!res.ok) {
            // create error instance with HTTP status text
            const error = new Error(res.statusText);
            error.json = res.json();
            throw error;
          }
          return res.json();
        })
        .then((json) => {
          // set the response data
          data.value = json;
        })
        .catch((err) => {
          error.value = err;
          // In case a custom JSON error response was provided
          if (err.json) {
            return err.json.then((json) => {
              // set the JSON response message
              error.value.message = json.message;
            });
          }
        })
        .then(() => {
          loading.value = false;
        });
    };

    onMounted(() => {
      fetchData();
    });

    return {
      data,
      loading,
      error,
      fetchData,
    };
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.beer {
  min-width: 350px;
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
}

.container {
  margin: 0 10px;
  max-width: 100%;
  align-items: center;
  justify-content: center;
  color: black;
  padding: 10px;
  border-radius: 10px;
  box-shadow: 10px 10px 4px rgba(45, 36, 102, 0.5);
  position: relative;
  background: #ffffff;
  opacity: 85%;
}

h4 {
  margin: 0;
  font-weight: 900;
}

.text__block {
  display: flex;
  font-weight: 600;
  margin-bottom: 10px;
}

.button {
  border-radius: 10px;
  background: linear-gradient(to right, #d3cce3, #e9e4f0);
  padding: 10px;
  border: none;
  box-shadow: 1px 1px 4px rgba(45, 36, 102, 0.5);
  margin: 15px;
}

.button:hover {
  color: #151e36;
  background: linear-gradient(45deg, #d299c2, #fef9d7);
  cursor: pointer;
}
</style>
