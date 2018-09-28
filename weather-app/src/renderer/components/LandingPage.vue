<template>
  <main id="app">
    <p>Entrez le nom de la ville</p>
    <section class="weather-app">
      <input type="text" v-model="query">
      <button :disabled="!query.length"  @click="showWeather">check</button>
    </section>

    <section v-if=" error" class="weather-error">
      Cette vile n'existe pas ! Rententez avec une autre ;)
    </section>

    <section v-if="city.length" class="weather result">
      <h1>{{city}}, {{country}}</h1>
      <p><em>{{weatherDescription}}</em></p>
      <div class="weather-result_main">
          <img :src="icon" alt="Weather icon">
          <div class="weather-result_temp">
              {{temp}}&deg;C
          </div>
      </div>
      <div class="weather-result_details">
        <p>Min: {{tempMin}}&deg;C</p>
        <p>Max: {{tempMax}}&deg;C</p>
        <p>Humidité: {{humidity}}%</p>
      </div>
    </section>

  </main>
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
    this.$http.get(`/weather?q=${this.query}&units=metric&&appid=${`7af6fab88c5d00b927eefc28d21ba4a5
`}`)
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
      });
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
    display: inline-flex;
    text-align: center;
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

<style lang="scss">
  * {
    margin: 0;
    padding: 0;
  }
  html,
  body,
  #app {
    height: 100%;
  }
  #app {
    font-family: Arial, Helvetica, sans-serif;
    font-size: 16px;
    padding: 10px;
    background: rgb(212, 228, 239);
    background: -moz-radial-gradient(
      center,
      ellipse cover,
      rgba(212, 228, 239, 1) 0%,
      rgba(134,174, 204, 1) 100%
    );
    filter: progid:DXImageTransform.Microsoft.gradient( startColorstr='#d4e4ef', endColorstr='#86aecc',GradientType=1);
  }

  .weather-input {
    display: flex;
    align-items: center;
    padding: 20px 0;
  }

  .weather-result {
    text-align: center;
    &__main {
      display: flex;
      align-items: center;
      justify-content: center;
      padding-top: 5px;
      font-size: 1.3rem;
      font-weight: bold;
    }
    &__details {
      display: flex;
      align-items: center;
      justify-content: center;
      color: dimgray;

    }
  }

  .weather-error {
    color: red;
    font-weight: bold;
  }

  input {
    width: 75%;
    outline: none;
    height: 20px;
    font-size: 0.8rem;
  }

  button {
    display: block;
    width: 25%;
    height: 25px;
    outline: none;
    border-radius: 5px;
    white-space: nowrap;
    margin: 0 10px;
    font-size: 0.8rem;
  }
</style>
