<template>
  <div id="app">
    <p>Entrez le nom de la ville</p>
    <section class="weather-app">
      <input type="text" v-model="query">
      <button :disabled="!query.lenght">check</button>
    </section>
  </div>
</template>

<script>
  import SystemInformation from './LandingPage/SystemInformation'

  export default {
    name: 'landing-page',
    data() {
      return {
        query: '',
        error: false,
        city: '',
        country: '',
        weatherDescription: '',
        temp: null,
        tempMin: null,
        tempMax: null,
        humidity: null,
        icon: '',
      }
    },
    methods: {
      showWeather() {
        this.$http
          .get(`/weather?q=${this.query}&units=metric&&appid=${API_KEY}`)
          .then(response => {
            this.city = response.data.name;
            this.country = response.data.sys.country;
            this.weatherDescription = response.data.weather[0].description;
            this.temp = response.data.main.temp;
            this.tempMin = response.data.main.temp_min;
            this.tempMax = response.data.main.temp_max;
            this.humidity = response.data.main.humidity;
            this.icon = `http://openweathermap.org/img/w/${
              response.data.weather[0].icon
            }.png`;
            this.error = false;
          })
          .catch(() => {
            this.error = true;
            this.city = '';
          })
        }
      },
  }
</script>

<style>
  @import url('https://fonts.googleapis.com/css?family=Source+Sans+Pro');

  * {
    box-sizing: border-box;
    margin: 0;
    padding: 0;
  }

  body { font-family: 'Source Sans Pro', sans-serif; }

  #wrapper {
    background:
      radial-gradient(
        ellipse at top left,
        rgba(255, 255, 255, 1) 40%,
        rgba(229, 229, 229, .9) 100%
      );
    height: 100vh;
    padding: 60px 80px;
    width: 100vw;
  }

  #logo {
    height: auto;
    margin-bottom: 20px;
    width: 420px;
  }

  main {
    display: flex;
    justify-content: space-between;
  }

  main > div { flex-basis: 50%; }

  .left-side {
    display: flex;
    flex-direction: column;
  }

  .welcome {
    color: #555;
    font-size: 23px;
    margin-bottom: 10px;
  }

  .title {
    color: #2c3e50;
    font-size: 20px;
    font-weight: bold;
    margin-bottom: 6px;
  }

  .title.alt {
    font-size: 18px;
    margin-bottom: 10px;
  }

  .doc p {
    color: black;
    margin-bottom: 10px;
  }

  .doc button {
    font-size: .8em;
    cursor: pointer;
    outline: none;
    padding: 0.75em 2em;
    border-radius: 2em;
    display: inline-block;
    color: #fff;
    background-color: #4fc08d;
    transition: all 0.15s ease;
    box-sizing: border-box;
    border: 1px solid #4fc08d;
  }

  .doc button.alt {
    color: #42b983;
    background-color: transparent;
  }
</style>
