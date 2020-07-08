<template>
  <div class="text-center mt-5 pb-5 container">
    <h1>Weather app</h1>
    <div class="md-form col-md-4 mx-auto mt-4 mb-3">
      <input
        id="search"
        type="text"
        class="form-control"
        placeholder="Search City..."
        v-model="searchInput"
        @keyup.enter="getWeather"
      />
      <button class="btn btn-block btn-white mt-4" type="submit" @click.prevent="getWeather">Search</button>
    </div>

    <div v-if="typeof weather.main != 'undefined'" class="result mt-5">
      <div class="city-output">
        <div class="city">
          <h1>{{ weather.name }}</h1>
        </div>
      </div>
      <div class="weather-output">
        <div class="weather">
          <p>{{ weather.weather[0].main }}</p>
        </div>
        <div class="temp">
          <p>{{ Math.round(weather.main.temp) }}°C</p>
        </div>
        <div class="weather">
          <img :src="iconUrl" alt="icon" class="icon" />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      apiKey: "2ac19d1b727b283e58cbd092a0c3a7ec",
      url: "https://api.openweathermap.org/data/2.5/",
      searchInput: "",
      weather: {},
      icon: "",
      iconUrl: ""
    };
  },
  methods: {
    getWeather() {
      fetch(
        `${this.url}weather?q=${this.searchInput}&units=metric&APPID=${this.apiKey}`
      )
        .then(res => res.json())
        .then(data => {
          this.weather = JSON.parse(JSON.stringify(data));
          this.icon = JSON.parse(JSON.stringify(data.weather[0].icon));
          this.iconUrl = `http://openweathermap.org/img/wn/${this.icon}@2x.png`;
        });
      this.searchInput = "";
    }
  }
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Roboto:wght@100;400;700&display=swap");

body {
  font-family: "Roboto", sans-serif;
  background-image: url('./assets/sunny.jpg');
  background-size: cover;
  background-repeat: no-repeat;
  height: 100vh;
  color: #fff;
}

.md-form {
  background-color: white;
  border-radius: 0.5rem;
  padding: 2rem;
}

.weather p {
  font-size: 1.4rem;
  margin-bottom: 0rem;
}

.temp p {
  font-size: 5rem;
  font-weight: 100;
  margin-bottom: 0rem;
}

</style>
