<template>
  <div
    id="app"
    :class="[typeof errorExist != 'undefined' && errorExist == true ? 'error' : '',
            typeof weather.consolidated_weather != 'undefined' && weather.consolidated_weather[0].weather_state_name == 'Light Rain' ? 'rain' : '',
            typeof weather.consolidated_weather != 'undefined' && weather.consolidated_weather[0].weather_state_name == 'Heavy Rain' ? 'rain' : '',
            typeof weather.consolidated_weather != 'undefined' && weather.consolidated_weather[0].weather_state_name == 'Showers' ? 'rain' : '',
            typeof weather.consolidated_weather != 'undefined' && weather.consolidated_weather[0].weather_state_name == 'Heavy Cloud' ? 'clouds' : '',
            typeof weather.consolidated_weather != 'undefined' && weather.consolidated_weather[0].weather_state_name == 'Light Cloud' ? 'clouds' : '',
            typeof weather.consolidated_weather != 'undefined' && weather.consolidated_weather[0].weather_state_name == 'Snow' ? 'snow' : '',
            typeof weather.consolidated_weather != 'undefined' && weather.consolidated_weather[0].weather_state_name == 'Sleet' ? 'snow' : '',
            typeof weather.consolidated_weather != 'undefined' && weather.consolidated_weather[0].weather_state_name == 'Hail' ? 'snow' : '',
            typeof weather.consolidated_weather != 'undefined' && weather.consolidated_weather[0].weather_state_name == 'Clear' ? 'clear' : ''
            ]"
  >
    <main v-bind:class="[typeof weather.consolidated_weather == 'undefined' ? 'centerDiv ' : '']">
      <div class="search-box">
        <div class="row">
          <div class="col-md-12 d-inline">
              <input
                  type="text"
                  name="text"
                  class="search-bar"
                  placeholder="Please write the city you want to know the weather..."
                  autocomplete="off"
                  v-model="query"
                  @keypress="startFetching"
              />
              <button v-on:click="fetchWeather">
                <i class="fa fa-search" aria-hidden="true"></i>
              </button>
            </div>
        </div>
      </div>
      <PulseLoader class="load" :loading="loading" color="white" ></PulseLoader>
      <div class="weather-wrap" v-if="typeof weather.consolidated_weather != 'undefined' ">
        <div class="location-box">
          <div class="pt-5"></div>
          <div class="location">{{ weather.title }}, {{ weather.location_type }}</div>
          <div class="date">{{ dateBuilder(0) }}</div>
        </div>
        <div class="weather-box">
          <div class="pt-3"></div>
          <div class="actualtemp">
            <img
                height="150" width="150"
                v-bind:src=" 'https://www.metaweather.com/static/img/weather/' + weather.consolidated_weather[0].weather_state_abbr + '.svg'"
            />
          </div>
          <div class="state">{{ weather.consolidated_weather[0].weather_state_name }}</div>
          <div class="temp">{{ Math.round(weather.consolidated_weather[0]['the_temp']) }}°C</div>
          <div class="pt-3"></div>
        </div>
        <div class="container">

          <div class="second">
            <div class="location-box">
              <div class="pt-5"></div>
              <div class="dateNext">{{ dateBuilder(1) }}</div>
            </div>
            <div class="weather-box">
              <div class="pt-3"></div>
              <div class="actualtemp">
                <img
                    eight="80" width="80"
                    v-bind:src=" 'https://www.metaweather.com/static/img/weather/' + weather.consolidated_weather[1].weather_state_abbr + '.svg'"
                />
              </div>
              <div class="stateNext">{{ weather.consolidated_weather[1].weather_state_name }}</div>
              <div class="tempNext">{{ Math.round(weather.consolidated_weather[1]['the_temp']) }}°C</div>
              <div class="pt-3"></div>
            </div>
          </div>
          <div class="third">
            <div class="location-box">
              <div class="pt-5"></div>
              <div class="dateNext">{{ dateBuilder(2) }}</div>
            </div>
            <div class="weather-box">
              <div class="pt-3"></div>
              <div class="actualtemp">
                <img
                    eight="80" width="80"
                    v-bind:src=" 'https://www.metaweather.com/static/img/weather/' + weather.consolidated_weather[2].weather_state_abbr + '.svg'"
                />
              </div>
              <div class="stateNext">{{ weather.consolidated_weather[2].weather_state_name }}</div>
              <div class="tempNext">{{ Math.round(weather.consolidated_weather[2]['the_temp']) }}°C</div>
              <div class="pt-3"></div>
            </div>
          </div>
          <div class="fourth">
            <div class="location-box">
              <div class="pt-5"></div>
              <div class="dateNext">{{ dateBuilder(3) }}</div>
            </div>
            <div class="weather-box">
              <div class="pt-3"></div>
              <div class="actualtemp">
                <img
                    height="80" width="80"
                    v-bind:src=" 'https://www.metaweather.com/static/img/weather/' + weather.consolidated_weather[3].weather_state_abbr + '.svg'"
                />
              </div>
              <div class="stateNext">{{ weather.consolidated_weather[3].weather_state_name }}</div>
              <div class="tempNext">{{ Math.round(weather.consolidated_weather[3]['the_temp']) }}°C</div>
              <div class="pt-3"></div>
            </div>
          </div>
          <div class="fifth">
            <div class="location-box">
              <div class="pt-5"></div>
              <div class="dateNext">{{ dateBuilder(4) }}</div>
            </div>
            <div class="weather-box">
              <div class="pt-3"></div>
              <div class="actualtemp">
                <img
                    eight="80" width="80"
                    v-bind:src=" 'https://www.metaweather.com/static/img/weather/' + weather.consolidated_weather[4].weather_state_abbr + '.svg'"
                />
              </div>
              <div class="stateNext">{{ weather.consolidated_weather[4].weather_state_name }}</div>
              <div class="tempNext">{{ Math.round(weather.consolidated_weather[4]['the_temp']) }}°C</div>
              <div class="pt-3"></div>
            </div>
          </div>

        </div>
      </div>
      <br>
      <div class="alert" v-if="typeof errorExist != 'undefined' && errorExist == true ">
        <span class="closebtn" onclick="this.parentElement.style.display='none';">&times;</span>
        {{ errorMessage}}
      </div>

    </main>
  </div>
</template>

var proxyUrl = 'https://cors-anywhere.herokuapp.com/',
targetUrl = 'http://catfacts-api.appspot.com/api/facts?number=99'
<script>
import PulseLoader from "@/PulseLoader";
export default {
  name: "App",
  components:
      {
        PulseLoader
      },
  data: () => ({
    url_base: "https://www.metaweather.com/api",
    proxyUrl : 'https://cors-anywhere.herokuapp.com/',
    query: "",
    geoInfo: "",
    weather: {},
    errorExist: false,
    loading: false,
    errorMessage:"",
    woeidError: false
  }),
  methods: {
    fetchWeather() {
      this.errorExist = false;
      this.woeidError = false;
      this.loading = true
      fetch(
          this.proxyUrl+`${this.url_base}/location/search/?query=${this.query}`
      )
        .then(res => res.json())
        .then(this.setGeoInfo)
        .then(this.getResults)
        .catch(error => {
          console.log(`ERROR ${error}`);
          this.errorExist = true;
          this.weather = {};
          this.loading = false;
          this.errorMessage = error;
        });
    },
    startFetching(e){
      if (e.key == "Enter") {
        this.fetchWeather()
      }
    },
    getResults(){
      fetch(
          this.proxyUrl+`${this.url_base}/location/${this.geoInfo}`
      )
          .then(res => res.json())
          .then(this.setResults)
          .catch(error => {
              console.log(`ERROR ${error}`);
              this.errorExist = true;
              this.weather = {};
              this.loading = false;
              this.errorMessage = error;
              this.woeidError = true;
            });
    },
    setGeoInfo(geoResults){
      if(this.errorExist)
        return ;
      else
        this.geoInfo = geoResults[0]['woeid'];
    },
    setResults(results) {
      this.weather = results;
      this.loading = false;
    },
    dateBuilder(number) {
      let d = new Date();
      d.setDate(d.getDate() + number);
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
      let date = d.getDate() ;
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
}
body {
  font-family: "Ubuntu", sans-serif;
}
#app {
  background-image: url("./assets/hallstat.jpg");
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}
#app.error {
  background-image: url("./assets/clear-bg.jpg");
  transition: 0.4s;
}

#app.rain {
  background-image: url("./assets/rain.gif");
  transition: 0.4s;
}
#app.clouds {
  background-image: url("./assets/clouds.gif");
  transition: 0.4s;
}
#app.clear {
  background-image: url("./assets/clear.gif");
  transition: 0.4s;
}
#app.snow {
  background-image: url("./assets/snow.gif");
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
.load{
  text-align: center;
  color: #8c0615;
}
.container {
  height: 100%;
  width: 100%;
  display: flex;
}
.second {
  width: 25%;
}
.third {
  width: 25%;
}
.fourth {
  width: 25%;
}
.fifth {
  width: 25%;
}


.search-box {
  padding: 5px;
  border-radius: 0.7rem;
  background-color: rgba(0, 0, 0, 0.8);
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
  width: 48.5%;
  margin-left: auto;
  margin-right: auto;
}
.search-box .search-bar {
  border-radius: 0.7rem;
  background: rgba(255, 255, 255, 0.2);
  border: none;
  padding: 5px;
  color: white;
  width: 97.0%;
  text-align: center;
}
.search-box button {
  border-radius: 1.4rem;
  padding: 5px;
  border: none;
  background-color: rgba(0, 0, 0, 0.8);
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
.weather-box .state {
  color: #fff;
  font-size: 32px;
  font-weight: 300;
  font-style: italic;
  text-align: center;
  padding-top: 5px;
}
.weather-box {
  text-align: center;
}
.weather-box .temp {
  display: inline-block;
  padding: 10px 25px;
  color: #fff;
  font-size: 62px;
  font-weight: 900;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color: rgba(0, 0, 0, 0.25);
  border-radius: 16px;
  margin: 30px 0px;
  box-shadow: 3px 6px rgba(0, 0, 0, 0.5);
}

.location-box .dateNext {
  color: #fff;
  font-size: 15px;
  font-weight: 300;
  font-style: italic;
  text-align: center;
}

.weather-box .tempNext {
  display: inline-block;
  padding: 10px 25px;
  color: #fff;
  font-size: 25px;
  font-weight: 900;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color: rgba(0, 0, 0, 0.25);
  border-radius: 16px;
  margin: 30px 0px;
  box-shadow: 3px 6px rgba(0, 0, 0, 0.5);
}
.weather-box .stateNext {
  color: #fff;
  font-size: 20px;
  font-weight: 300;
  font-style: italic;
  text-align: center;
  padding-top: 5px;
}

.alert{
  padding: 20px;
  background-color: #f44336;
  color: #fff;
  opacity: 1;
  transition: opacity 0.6s;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
  margin-bottom: 15px;
  border-radius: 1.0rem;
  width: 48.5%;
  margin-left: auto;
  margin-right: auto;
  text-align: center;
}

/* The close button */
.closebtn {
  margin-left: 15px;
  color: white;
  font-weight: bold;
  float: right;
  font-size: 22px;
  line-height: 20px;
  cursor: pointer;
  transition: 0.3s;
}

/* When moving the mouse over the close button */
.closebtn:hover {
  color: black;
  opacity: 0;
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
</style>