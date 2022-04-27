<template>
  <div className="user">
    <div className="container" v-if="!loading && data">
      <div className="front">
        <img
          className="card__img"
          :src="data.avatar"
          alt="avatar"
          @error="defaultAvatar"
        />
        <div className="card__name">
          {{ data.first_name }} {{ data.last_name }}
        </div>
        <div className="card__text">Должность: {{ data.employment.title }}</div>
      </div>
      <div className="back">
        <div className="card__text">
          <h4>Email:</h4>
          {{ data.email }}
        </div>
        <div className="card__text">
          <h4>Телефон:</h4>
          {{ data.phone_number }}
        </div>
        <div className="card__text">
          <h4>Дата рождения:</h4>
          {{ data.date_of_birth }}
        </div>
        <div className="card__text">
          <h4>Адрес:</h4>
          {{ data.address.zip_code }} {{ data.address.country }}
          {{ data.address.state }} {{ data.address.city }}
          {{ data.address.street_name }}
          {{ data.address.street_address }}
        </div>
      </div>
    </div>
    <p v-if="loading">Loading...</p>
  </div>
</template>

<script>
import { ref, onMounted } from "vue";
import img from "@/assets/avatar.png";

export default {
  name: "UserComponent",
  props: {},
  methods: {
    defaultAvatar(e) {
      e.target.src = img;
    },
  },
  setup() {
    const data = ref(null);
    const loading = ref(true);
    const error = ref(null);

    function fetchData() {
      loading.value = true;
      return fetch("https://random-data-api.com/api/users/random_user")
        .then((res) => {
          if (!res.ok) {
            const error = new Error(res.statusText);
            error.json = res.json();
            throw error;
          }
          return res.json();
        })
        .then((json) => {
          data.value = json;
        })
        .catch((err) => {
          error.value = err;
          if (err.json) {
            return err.json.then((json) => {
              error.value.message = json.message;
            });
          }
        })
        .then(() => {
          loading.value = false;
        });
    }

    onMounted(() => {
      fetchData();
    });

    return {
      data,
      loading,
      error,
    };
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.user {
  width: 300px;
  background: #ffffff;
  padding: 20px;
  border-radius: 10px;
  margin: 5px;
}

.container {
  position: relative;
}

.card__img {
  width: 100%;
  margin-bottom: 10px;
  border-radius: 10px;
  background: #939191;
}

.card__item .front {
  backface-visibility: hidden;
  transition: all 0.5s ease;
}

.card__item .back {
  position: absolute;
  top: 0;
  left: 0;
  padding: 10px;
  transform: rotateY(180deg);
  backface-visibility: hidden;
  transition: all 0.5s ease;
}

.card__item:hover .front {
  transform: rotateY(-180deg);
}

.card__item:hover .back {
  transform: rotateY(0);
}

.card__name {
  font-size: 18px;
  font-weight: 600;
  margin-bottom: 10px;
}

h4 {
  margin: 0;
}

.card__text {
  font-size: 14px;
  margin-bottom: 10px;
}

@media screen and (max-width: 900px) {
  .user {
    padding: 20px;
  }
}
</style>
