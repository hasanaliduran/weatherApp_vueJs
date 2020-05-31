<template>
  <div
    id="app"
    :class="[typeof weather.main != 'undefined' && weather.weather[0].main == 'Rain' ? 'rain' : '',
            typeof weather.main != 'undefined' && weather.weather[0].main == 'Clouds' ? 'clouds' : '',
            typeof weather.main != 'undefined' && weather.weather[0].main == 'Clear' ? 'clear' : ''
            ]"
  >
    <main v-bind:class="[typeof weather.main == 'undefined' ? 'centerDiv' : '']">
      <div class="search-box">
        <div class="row">
          <div class="col-md-12 d-inline">
            <input
              type="text"
              name="text"
              class="search-bar"
              placeholder="Search..."
              autocomplete="off"
              v-model="query"
              @keypress="fetchWeather"
            />
            <button>
              <i class="fa fa-search" aria-hidden="true"></i>
            </button>
          </div>
        </div>
      </div>
      <div class="weather-wrap" v-if="typeof weather.main != 'undefined' ">
        <div class="location-box">
          <div class="pt-5"></div>
          <div class="location">{{ weather.name }}, {{ weather.sys.country }}</div>
          <div class="date">{{ dateBuilder() }}</div>
        </div>
        <div class="weather-box">
          <div class="pt-3"></div>
          <div class="actualtemp">
            <img
              v-bind:src=" 'http://openweathermap.org/img/wn/' + weather.weather[0].icon + '.png'"
            />
          </div>
          <div class="temp">{{ Math.round(weather.main.temp) -272 }}°C</div>
          <div class="weather">{{ weather.weather[0].main }}</div>
          <div class="pt-3"></div>
        </div>
      </div>
      <div v-else class="search-box">
        <p class="pt-2 text-white text-center font-weight-bold">Provide a city name.</p>
      </div>
    </main>
  </div>
</template>

<script>
export default {
  name: "App",
  data: () => ({
    api_key: "e4491ae2bcc33e7751609d50383eaf9a",
    url_base: "https://api.openweathermap.org/data/2.5",
    query: "",
    weather: {}
  }),
  methods: {
    fetchWeather(e) {
      if (e.key == "Enter") {
        fetch(
          `${this.url_base}/weather?q=${this.query}&appid=${this.api_key}&units=metrics`
        )
          .then(res => res.json())
          .then(this.setResults);
      }
    },
    setResults(results) {
      this.weather = results;
    },
    dateBuilder() {
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
@import url("https://fonts.googleapis.com/css2?family=Ubuntu&display=swap");

* {
  outline: 0;
  padding: 0;
  margin: 0;
  box-sizing: border-box;
  overflow: hidden;
}
body {
  font-family: "Ubuntu", sans-serif;
}
#app {
  background-image: url("./assets/warm-bg.jpg");
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}
#app.rain {
  background-image: url("./assets/rain-bg.jpg");
  transition: 0.4s;
}
#app.clouds {
  background-image: url("./assets/clouds-bg.jpg");
  transition: 0.4s;
}
#app.clear {
  background-image: url("./assets/clear-bg.jpg");
  transition: 0.4s;
}
main {
  transition: all 0.4s ease;
  min-height: 100vh;
  padding: 25px;
  background-image: linear-gradient(
    to bottom,
    rgba(0, 0, 0, 0.25),
    rgba(0, 0, 0, 0.75)
  );
}
.centerDiv {
  padding-top: 15rem;
  transition: all 0.4s ease;
}
.search-box {
  padding: 10px;
  background-color: rgba(0, 0, 0, 0.8);
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
}
.search-box .search-bar {
  background: rgba(255, 255, 255, 0.2);
  border: none;
  padding: 5px;
  color: white;
  width: 98.5%;
}
.search-box button {
  padding: 5px;
  border: none;
  background: rgba(255, 255, 255, 0.2);
  color: white;
  float: right;
  position: absolute;
}
.search-box button:hover {
  color: #cccccc;
  transition: all 0.4s ease;
}
.search-box button:focus {
  outline: 0;
  color: #8a8a8a;
  transition: all 0.4s ease;
}
.location-box .location {
  color: #fff;
  font-size: 32px;
  font-weight: 500;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
  text-align: center;
}
.weather-box .actualtemp {
  color: #fff;
  font-size: 22px;
  font-weight: 500;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
  text-align: center;
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
.weather-box .temp {
  display: inline-block;
  padding: 10px 25px;
  color: #fff;
  font-size: 102px;
  font-weight: 900;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color: rgba(0, 0, 0, 0.25);
  border-radius: 16px;
  margin: 30px 0px;
  box-shadow: 3px 6px rgba(0, 0, 0, 0.5);
}
.weather-box .weather {
  color: #fff;
  font-size: 48px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
@media only screen and (min-width: 768px) and (max-width: 1100px) {
  .search-box .search-bar {
    width: 97%;
  }
}
@media only screen and (min-width: 521px) and (max-width: 767px) {
  .search-box .search-bar {
    width: 96%;
  }
}
@media only screen and (max-width: 520px) {
  .search-box .search-bar {
    width: 94%;
  }
}
@media only screen and (max-width: 767px) {
  #app.rain {
    background-image: url("./assets/rain.gif");
  }
  #app.clouds {
    background-image: url("./assets/clouds.gif");
  }
  #app.clear {
    background-image: url("");
  }
}
</style>
