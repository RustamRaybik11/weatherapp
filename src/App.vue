<template>
  <div id="app" :class="[typeof weather.main != 'undefined' && weather.main.temp > 26 ? 'hot' : '',  
    typeof weather.main != 'undefined' && weather.main.temp > 15 &&  weather.main.temp < 26? 'warm' : '',  
    typeof weather.main != 'undefined' && weather.main.temp > 0 &&  weather.main.temp < 16? 'cold' : '',
    typeof weather.main != 'undefined' && weather.main.temp < 1? 'verycold' : '']">
    <main>
      <div class="search-box">
        <input 
          type="text" 
          class="search-bar" 
          placeholder="Search..."
          v-model="query"
          @keypress="fetchWeather"
        />
      </div>

      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location">{{ weather.name }}, {{ weather.sys.country }}</div>
          <div class="date">{{ dateBuilder() }}</div>
        </div>

        <div class="weather-box">
          <div class="temp">{{ Math.round(weather.main.temp) }}°c</div>
          <div class="weather">{{ weather.weather[0].main }}</div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
export default {
  name: 'app',
  data () {
    return {
      api_key: '073eff7d72b7ed292594a6133bf15d90',
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
*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
body{
  font-family: 'montserrat', sans-serif;
}
#app {
  text-align: center;
  background-image: url(./assets/bg.png);
  background-position: bottom;
  transition: 0.4s;
  background-position: center;
  max-width: 1920px;
}
#app.hot{
  background-image: url(./assets/hot.png);
}
#app.warm{
  background-image: url(./assets/warm.png);
}
#app.cold{
  background-image: url(./assets/cold.png);
}
#app.verycold{
  background-image: url(./assets/verycold.png);
}
main{
  min-height: 100vh;
  padding: 25px;

  background-image: linear-gradient(to bottom, rgba(0, 0, 0, 0.25), rgba(0, 0, 0, 0.75));
}
.search-box{
  width: 100%;
  margin-bottom: 30px;
}
.search-box .search-bar{
  display: block;
  width: 100%;
  padding: 15px; 

  color: #313131;
  font-size: 20px;

  appearance: none;
  border: none;
  outline: none;
  background: none;

  box-shadow: 0, 0, 8px rgba(0, 0, 0, 0.25);
  background-color: rgb(255, 255, 255, 0.5);
  border-radius: 16px 0px 16px 0px;
  transition: 0.4s;
}
.search-box .search-bar:focus{
  box-shadow: 0, 0, 16px rgba(0, 0, 0, 0.25);
  background-color: rgb(255, 255, 255, 0.75);
  border-radius: 0px 16px 0px 16px;
}
.location-box .location{
  color: #fff;
  font-size: 28px;
  font-weight: 500;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}
.location-box .date{
  color: #fff;
  font-size: 20px;
  font-weight: 300;
  font-style: italic;
}
.weather-box .temp{
  display: inline-block;
  padding: 10px 25px;
  color: #fff;
  font-size: 72px;
  font-weight: 700;

  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color: rgba(0, 0, 0, 0.25);
  border-radius: 16px;
  margin: 30px 0;

  box-shadow: rgba(0, 0, 0, 0.25);
}
.weather-box .weather{
  color: #fff;
  font-size: 48px;
  font-weight: 600;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
</style>
