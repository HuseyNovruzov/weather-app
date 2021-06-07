<template>
  <div id="app">
    <img src="./assets/warm.jpg" alt="" class="land-image" v-if="isWarmTemp()">
    <img src="./assets/cold.jpg" alt="" class="land-image" v-if="!isWarmTemp()">
    <main>
      <div class="search-box">
        <input
          type="text"
          class="search-bar"
          placeholder="Search..."
          v-model="query"
          @keypress.enter="fetchData"
        />
      </div>
      <div class="weather-wrap" v-if="isUndefined()">
        <div class="location-box">
          <div class="location">
            {{ weather.name }} {{ weather.sys.country }}
          </div>
          <div class="date">{{ buildDate() }}</div>
        </div>
        <div class="temp-box">
          <div class="temp">
            {{ Math.round(weather.main.temp - 273.15) }} °C
          </div>
          <div class="mod">{{ weather.weather[0].main }}</div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      query: "",
      api_key: "091c45c1bc8164df49f8eadd39e79262",
      url_base: "https://api.openweathermap.org/data/2.5",
      weather: {},
    };
  },
  methods: {
    fetchData() {
      fetch(`${this.url_base}/weather?q=${this.query}&appid=${this.api_key}`)
        .then((res) => { return res.json(); })
        .then((res) => { this.weather = res; });
    },
    isUndefined() {
      if (typeof this.weather.main !== "undefined") {
        return true;
      }
      return false;
    },
    isWarmTemp() {
      const valid = this.isUndefined();
      if (valid) {
        const greather = Math.round(this.weather.main.temp - 273.15);
        if (greather > 16) {
          return true;
        }
      }
      return false;
    },
    buildDate() {
      const d = new Date();
      const days = [
        "Monday",
        "Tuesday",
        "Wednesday",
        "Thursday",
        "Friday",
        "Saturday",
        "Sunday",
      ];
      const months = [
        "January",
        "February",
        "March",
        "April",
        "May",
        "June",
        "July",
        "August",
        "September",
        "October",
        "November",
        "December",
      ];
      const day = days[d.getDay()];
      const date = d.getDate();
      const month = months[d.getMonth()];
      const year = d.getFullYear();
      return `${day} ${date} ${month} ${year}`;
    },
  },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
#app {
  background-position: bottom;
  background-size: cover;
  transition: 0.4s;
}
.land-image{
  width: 100%;
  height: 100%;
  position: absolute;
  z-index: -1;
  object-fit: cover;
}
main {
  min-height: 100vh;
  padding: 10px;
}
.search-box {
  width: 100%;
}
.search-bar {
  display: block;
  outline: none;
  border: none;
  width: 300px;
  padding: 10px;
  background: none;
  background-color: rgba(212, 204, 204, 0.8);
  border-radius: 8px 0;
  transition: 0.4s;
  color: #000;
  font-weight: 500;
}
.search-bar:focus {
  background-color: rgba(247, 236, 236, 0.85);
  border-radius: 0 8px;
}
.weather-wrap {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-top: 10px;
}
.weather-wrap .location {
  color: rgb(99, 97, 97);
  font-size: 25px;
  font-weight: 500;
  text-align: center;
  margin-bottom: 20px;
}
.weather-wrap .date {
  color: rgb(99, 97, 97);
  font-size: 20px;
  font-style: italic;
  font-weight: 500;
  text-align: center;
  margin-bottom: 20px;
}
.temp-box .temp {
  background-color: rgba(255, 255, 255, 0.7);
  padding: 10px;
  font-size: 20px;
  border-radius: 10px;
  margin-bottom: 20px;
  color: rgb(99, 97, 97);
}
.temp-box .mod {
  text-align: center;
  font-size: 20px;
  color: rgb(99, 97, 97);
}
</style>
