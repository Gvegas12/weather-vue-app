<template>
  <div class="weather" :class="weatherClass">
    <div class="container">
      <div class="card weather-form">
        <input
          placeholder="Enter City"
          type="text"
          class="weather-form__input"
          v-model="searchQuery"
          @keyup.enter="weatherSearch"
        />
        <button @click="weatherSearch" class="weather-form__btn">Search</button>
      </div>

      <div class="card weather-load" v-if="loading">Loading...</div>

      <div class="weather-info" v-show="!error && !!location && !!temperature">
        <div v-if="errorMessage" class="weather-info__error-message card">
          {{ errorMessage }}
        </div>

        <div class="weather-info__text">
          <p class="card">{{ location }}</p>
          <p class="card">{{ temperature }} °C</p>
          <p class="card">{{ description }}</p>
        </div>
      </div>
    </div>

    <div class="weather-bg">
      <div>
        <img
          class="weather-bg__img bg"
          src="./assets/default_bg.jpg"
          alt="App Background"
        />
        <img
          class="weather-bg__img cloudy"
          src="./assets/cloudy.jpg"
          alt="Cloudy App Background"
        />
        <img
          class="weather-bg__img overcast"
          src="./assets/overcast.jpg"
          alt="Overcast App Background"
        />
        <img
          class="weather-bg__img sunny"
          src="./assets/sunny.jpg"
          alt="Sunny App Background"
        />
        <img
          class="weather-bg__img snow"
          src="./assets/snow.jpg"
          alt="Snow App Background"
        />
        <img
          class="weather-bg__img rain"
          src="./assets/rain.jpg"
          alt="Rain App Background"
        />
        <img
          class="weather-bg__img mist"
          src="./assets/mist.jpg"
          alt="Mist App Background"
        />
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      location: "",
      temperature: "-",
      description: "",
      loading: false,
      error: false,
      searchQuery: "",
      errorMessage: "",
    };
  },
  computed: {
    weatherClass() {
      if (this.description.includes("Partly cloudy")) {
        return "cloudy";
      }
      if (this.description.includes("Sunny")) {
        return "sunny";
      }
      if (this.description.includes("Overcast")) {
        return "overcast";
      }
      if (this.description.includes("Rain")) {
        return "rain";
      }
      if (this.description.includes("snow")) {
        return "snow";
      }
      if (this.description.includes("Mist")) {
        return "mist";
      }
      return "";
    },
  },
  methods: {
    weatherSearch() {
      this.loading = true;
      this.error = false;
      const API_KEY = "";

      fetch(
        `http://api.weatherapi.com/v1/current.json?key=${API_KEY}&q=${this.searchQuery}`
      )
        .then((response) => response.json())
        .then((data) => {
          this.loading = false;
          this.location = data.location.name;
          this.temperature = data.current.temp_c;
          this.description = data.current.condition.text;
          this.resetWeatherSearch();
        })
        .catch((error) => {
          console.error("Error fetching weather data:", error);
          this.error = true;
          this.errorMessage = error.message;
          this.loading = false;
        });
    },
    resetWeatherSearch() {
      this.searchQuery = "";
      this.errorMessage = "";
    },
  },
};
</script>
