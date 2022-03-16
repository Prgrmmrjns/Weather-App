<template>
  <div id="app">
    <main>
      <div class="title">
        <h1>Weather App</h1>
      </div>
      <div class="search-box">
        <input 
          type="text" 
          class="search-bar" 
          placeholder="Search weather..."
          v-model="query"
          @keypress="fetchWeather"
        />
      </div>

      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="info-box">
                  <div class="location-box">
          <div class="location">{{ weather.name }}, {{ weather.sys.country }}</div>
          <div class="date">{{ dateBuilder() }}</div>
        </div>
        <div class="weather-box">
          <div class="temp">{{ Math.round(weather.main.temp) }}°c</div>
          <div class="weather">{{ weather.weather[0].main }}</div>
        </div>
        </div>
        <div v-if="weather.weather[0].main  ==='Clear'">
            <img class="weather-img" src="./assets/clear.jpeg" alt="Clear sky">
          </div>
          <div v-else-if="weather.weather[0].main  ==='Rain'">
            <img class="weather-img" src="./assets/rain.jpeg" alt="Rainy weather">
          </div>
          <div v-else-if="weather.weather[0].main  ==='Clouds'">
            <img class="weather-img" src="./assets/clouds.webp" alt="Cloudy sky">
          </div>
          <div v-else-if="weather.weather[0].main  ==='Snow'">
            <img class="weather-img" src="./assets/snow.jpeg" alt="Snow fall">
          </div>
          <div v-else-if="weather.weather[0].main  ==='Extreme'">
            <img class="weather-img" src="./assets/extreme.jpeg" alt="Extreme weather">
          </div>
      </div>
    
      <p class="footer"> © {{new Date().getFullYear() }} - Jonas Wolber - Made in 🇪🇺</p>
      </main>
  </div>
</template>

<script>
export default {
  name: 'app',
  data () {
    return {
      api_key: 'a5fb0861c5bc7dc1c5e4be09b6421f28',
      url_base: 'https://api.openweathermap.org/data/2.5/',
      query: '',
      weather: {}
    }
  },
  methods: {
    fetchWeather (e) {
      if (e.key == "Enter") {
        fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
          .then(res => {
            return res.json();
          }).then(this.setResults);
      }
    },
    setResults (results) {
      this.weather = results;
    },
    dateBuilder () {
      let d = new Date();
      let months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];
      let days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"];
      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();
      return `${day} ${date} ${month} ${year}`;
    }
  }
}
</script>

<style>


* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
body {
  font-family: 'montserrat', sans-serif;
}
#app {
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}
main {
  min-height: 100vh;
  padding: 25px;
  background-image: linear-gradient(to bottom, rgba(0, 0, 0, 0.25), rgba(0, 0, 0, 0.75));
}
.title{
  
  text-align: center;
  margin: 50px;
  color: #393E46;
  padding: 15px;
  border-radius: 16px;
  font-size: 2rem;
  
}
.search-box {
  width: 100%;
  margin-bottom: 30px;
}
.search-box .search-bar {
  display: block;
  width: 100%;
  padding: 15px;
  
  color: #313131;
  font-size: 20px;
  appearance: none;
  border:none;
  outline: none;
  background: none;
  box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.5);
  border-radius: 0px 16px 0px 16px;
  transition: 0.4s;
}
.search-box .search-bar:focus {
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.75);
  border-radius: 16px 0px 16px 0px;
}
.weather-wrap{
  display: flex;
}
.info-box{
  margin-top: 50px;
  margin-left: 100px;
}

.location-box .location {
  color: #FFF;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
  margin-bottom: 10px;
}
.location-box .date {
  color: #FFF;
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
  color: #FFF;
  font-size: 102px;
  font-weight: 900;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color:rgba(0, 255, 255, 0.25);
  border-radius: 16px;
  margin: 30px 0px;
  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
.weather-box .weather {
  color: #FFF;
  font-size: 48px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
.weather-img{
  height: 550px;
  margin-left: 550px;
  margin-top: 25px;
  border-radius: 16px;
}
.footer{
  text-align: center;
  position: fixed; 
  bottom: 8px;
  left: 824px;
  font-family: 'Lora', serif;
}
</style>