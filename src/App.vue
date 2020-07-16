<template>
  <div id="app" :class="typeof weather.main != 'undefined' && weather.main.temp < 16 ? 'cold' : ''">
    <main>
      <div class="search-box">
        <input
          type="text"
          class="search-input"
          placeholder="Search..."
          v-model="query"
          @keypress="fetchWeather"
        />
      </div>

      <div class="weather-wrap" v-if="weather.main">
        <div class="location-box">
          <div class="location">{{ weather.name }}, {{ weather.sys.country }}</div>
          <div class="date">{{ getDate() }}</div>
        </div>

        <div class="weather-box">
          <div class="temp">{{ Math.round(weather.main.temp)}}°C</div>
          <div class="weather">{{ weather.weather[0].description }}</div>
        </div>
      </div>

      <div class="status-box" v-if="start">
        <div class="status">No location</div>
      </div>
    </main>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      api_key: "22e1e3225479d42a6ff53f332147999f",
      url_base: "https://api.openweathermap.org/data/2.5/",
      query: "",
      weather: {},
      start: true
    };
  },
  methods: {
    fetchWeather(e) {
      if (e.key == "Enter") {
        this.start = false;
        fetch(
          `${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`
        )
          .then(res => {
            return res.json();
          })
          .catch(err => alert(err))
          .then(this.setResults);
      }
    },
    setResults(results) {
      this.weather = results;
    },
    getDate() {
      let d = new Date();
      let months = [
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
        "December"
      ];
      let days = [
        "Sunday",
        "Monday",
        "Tuesday",
        "Wednesday",
        "Thursday",
        "Friday",
        "Saturday"
      ];

      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();

      return `${day} ${date} ${month} ${year}`;
    }
  }
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: "montserrat", sans-serif;
}

#app {
  background-image: url("./assets/clear.jpg");
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}

#app.cold {
  background-image: url("./assets/salji.jpg");
}

main {
  height: 100vh;
  padding: 25px;
  background-image: linear-gradient(
    to bottom,
    rgba(0, 0, 0, 0.25),
    rgba(0, 0, 0, 0.75)
  );
}

.search-box {
  width: 100%;
  margin-bottom: 200px;
}

.search-box .search-input {
  display: block;
  width: 100%;
  padding: 15px;
  color: #313131;
  font-size: 20px;

  appearance: none;
  border: none;
  outline: none;
  background: none;

  box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.5);
  transition: 0.4s;
}

.search-box .search-input:focus {
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.75);
  border-radius: 16px 16px 16px 16px;
}

.location-box .location {
  color: #fff;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}

.location-box .date {
  color: #fff;
  font-size: 20px;
  font-weight: 300;
  font-style: italic;
  text-align: center;
}

.weather-box {
  text-align: center;
}

.weather-wrap {
  transition: 0.3s;
}

.weather-box .temp {
  display: inline-block;
  padding: 10px 25px;
  color: #fff;
  margin: 30px 0;
  font-size: 102px;
  font-weight: 900;
  border-radius: 16px;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.25);
  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

.weather-box .weather {
  color: #fff;
  font-size: 48px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  transition: 0.3s;
}
.status-box{
  text-align: center;
  margin-top: 280px;
}
.status-box .status {
  color: #fff;
  font-size: 48px;
  font-weight: 700;
  margin-top: 100px;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  transition: 0.3s;
}

@media only screen and (max-width: 600px) {
.search-box {
  width: 100%;
  margin-bottom: 100px;
}
}
</style>
