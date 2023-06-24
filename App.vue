<template>
  <div id="app" :class="typeof weather.main != 'undefined' && weather.main.temp > 24 ? 'warm' : ''">
    <main>
      <div class="search-box">
        <input type="text" class="search-bar" placeholder="City Name" v-model="query" @keypress.enter="fetchWeather()"/>
      </div>

      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location"> {{weather.name}}, {{weather.sys.country}} </div>
          <div class="date"> {{getDate()}} <br> {{getDay()}} </div>
        </div>

        <div class="weather-box">
          <div class="temperature"> {{Math.round(weather.main.temp)}}°C </div>
          <div class="type"> {{weather.weather[0].main}} </div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>

export default {
  name: 'App',
  data() {
    return {
      api_key: 'ae638e144bef6826617cdae9bf42b110',
      baseURL: 'https://api.openweathermap.org/data/2.5/',
      query: '',
      weather: {}
    }
  },
  mounted() {
    let decodedCookie = decodeURIComponent(document.cookie);
    if (decodedCookie !== "") {
      this.query = decodedCookie;
      this.fetchWeather();
    }
    else
    {
      this.query = "London, GB"
      this.fetchWeather();
    }
  },
  methods: {
    fetchWeather() {
      fetch(`${this.baseURL}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
        .then(result => {
          return result.json();
        }).then(this.setResult);
    },

    setResult(result) {
      this.weather = result;
      document.cookie = `${this.weather.name}, ${this.weather.sys.country}`
    },

    getDate() {
      let d = new Date();
      let months = ['January', 'February', 'March', 'April', 'May', 'June', 'July', 'August', 'September', 'October', 'November', 'December'];

      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();

      return `${month} ${date}, ${year}`
    },
    getDay() {
      let d = new Date();
      let days = ['Sunday', 'Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday']
      let day = days[d.getDay()];

      return `${day}`
    }
  }
}
</script>

<style>

  *
  {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
  }

  body
  {
    font-family: 'Montserrat', sans-serif;
  }

  #app
  {
    background-image: url("./assets/cold.jpg");
    background-size: cover;
    background-position: bottom;
    transition: 0.5s;
  }
  #app.warm
  {
    background-image: url("./assets/warm.jpg");
  }

  main
  {
    min-height: 100vh;
    padding: 25px;

    background-image: linear-gradient(to bottom, rgba(0, 0, 0, 0.0625), rgba(0, 0, 0, 0.75));
  }

  .search-box
  {
    width: 100%;
    margin-bottom: 32px;
  }

  .search-box .search-bar
  {
    width: 100%;
    display: block;
    padding: 16px;

    color: #222222;
    font-size: 20px;
    font-family: 'Lato', 'Helvetica Neue Medium Extended', sans-serif;
    font-weight: 400;
    appearance: none;
    border: none;
    outline: none;
    background: none;
    background-color: rgba(255, 255, 255, 0.5);

    border-radius: 20px 4px 20px 4px;
    transition: 0.25s;
  }
  .search-box .search-bar:focus
  {
    border-radius: 4px 20px 4px 20px;
    box-shadow: 4px 4px 16px rgba(0, 0, 0, 0.25);
    background-color: rgba(255, 255, 255, 0.75)
  }

  .location-box .location
  {
    color: #ffffff;
    font-size: 32px;
    font-weight: 600;
    text-align: center;
    text-shadow: 2px 3px rgba(0, 0, 0, 0.25);
  }

  .location-box .date
  {
    color: #ffffff;
    font-size: 16px;
    font-weight: 200;
    text-align: center;
  }

  .weather-box
  {
    text-align: center;
  }

  .weather-box .temperature
  {
    display: inline-block;
    padding: 12px 24px;
    color: #ffffff;

    font-size: 96px;
    font-weight: 800;
    text-shadow: 4px 6px rgba(0, 0, 0, 0.25);
    font-family: 'Roboto', sans-serif;

    background-color: rgba(255, 255, 255, 0.25);
    border-radius: 24px;
    margin: 30px 0px;
    box-shadow: 4px 8px rgba(0, 0, 0, 0.125);
  }

  .weather-box .type
  {
    color: #ffffff;

    font-size: 48px;
    font-weight: 600;
    text-shadow: 3px 5px rgba(0, 0, 0, 0.25);
  }
</style>
