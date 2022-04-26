<template>
  <div className="user">
    <div className="container" v-if="!loading && data">
      <div className="card__wrapper">
        <div className="card__block">
          <a href="#" className="card__item">
            <div className="front">
              <img
                className="card__img"
                src="@/components/image/avatar.png"
                alt="avatar"
              />
              <div className="card__name">
                {{ data.first_name }} {{ data.last_name }}
              </div>
              <div className="card__text">
                Должность: {{ data.employment.title }}
              </div>
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
          </a>
        </div>
      </div>
    </div>
    <p v-if="loading">Loading...</p>
  </div>
</template>

<script>
import { ref, onMounted } from "vue";

export default {
  name: "UserComponent",
  props: {},
  setup() {
    const data = ref(null);
    const loading = ref(true);
    const error = ref(null);

    function fetchData() {
      loading.value = true;
      return fetch("https://random-data-api.com/api/users/random_user", {
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
  min-width: 300px;
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
}

.container {
  padding: 0 15px;
  margin: 0 30px;
}

.card__wrapper {
  display: flex;
  margin: 0 -15px;
}

.card__block {
  width: 100%;
  padding: 0 15px;
}

.card__item {
  display: block;
  text-decoration: none;
  color: black;
  padding: 20px;
  border-radius: 10px;
  box-shadow: 10px 5px 4px rgba(45, 36, 102, 0.5);
  position: relative;
  background: #ffffff;
  opacity: 85%;
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

.card__img {
  width: 100%;
  align-items: center;
  margin-bottom: 10px;
  object-fit: cover;
  display: flex;
  border-radius: 10px;
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
