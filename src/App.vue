<template>
  <div id="app">
    <main>
      <header>
        Weather App
      </header>
      <div class="search-container">
        <input type="text" class="search-bar" placeholder="Search" @keypress="getWeather" v-model="query">
      </div>

      <div class="weather-container" v-if="typeof weather.main != 'undefined'">
        <div class="weather-box">
          <div class="location">{{weather.name}}, {{weather.sys.country}}</div>
          <div class="temp">{{Math.round(weather.main.temp)}}*C</div>
          <div class="weather">{{weather.weather[0].main}}</div>
        </div>

        <div class="weather-image">
          <img v-bind:src="weather_images.sun" alt="">
        </div>
      </div>

    </main>
  </div>
</template>

<script>

export default {
  name: 'App',
  data () {
    return {
      api_key: '2f25834ac09b5895d9a3c7b5333a7faf',
      base_url: 'https://api.openweathermap.org/data/2.5/',
      query: '',
      weather: '',
      weather_images: {
        sun: 'https://media.giphy.com/media/o7R0zQ62m8Nk4/source.gif',
        rain: 'https://media.giphy.com/media/3osxYzIQRqN4DOEddC/source.gif',
      },
    }
  },
  methods: {
    async getWeather (e) {
      if(e.key == 'Enter'){
        try {
          const response = await fetch(`${this.base_url}weather?q=${this.query}&units=metric&APPID=${this.api_key}`);
          const data = await response.json();
          console.log(data);
          this.weather = data;
        }catch(err){
          console.error(err);
        }

      }
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
  font-family: 'Noto Serif JP', serif;
}

#app {
  --primary-text: #fff;
}

main {
  height: 100vh;
  padding: 24px;
  background-image: linear-gradient(hsl(230, 19%, 20%),hsl(230, 19%, 40%));
  color: var(--primary-text);
}

header {
  font-weight: bold;
  font-size: 48px;
  text-align: center;
  margin-bottom: 36px;
  text-shadow: 4px 6px rgba(0,0,0,.5);
}

.search-container {
  margin-bottom: 24px;
}

.search-container .search-bar{
  width: 100%;
  font-size: 24px;
  padding: 12px;
  border: none;
  background-color: rgba(0,0,0,.2);
  color: hsl(0, 0%, 71%);
  transition: .4s;
}

.search-container .search-bar:focus{
  background-color: rgba(0,0,0,.5);
  outline: none;
}

.weather-box {
  background-color: rgba(0,0,0,.2);
  padding: 24px 0;
  text-align: center;
  margin-bottom: 24px;
}

.weather-box > * {
  transition: opacity .4s;
}

.weather-container > *:hover {
  opacity: .5;
}

.weather-container .location {
  font-size: 36px;
  text-shadow: 4px 6px rgba(0,0,0,.5);
  margin-bottom: 12px;
}

.weather-container .temp {
  margin-bottom: 12px;
  font-size: 24px;
  color: hsl(0, 0%, 71%);
}

.weather-container .weather {
  font-size: 24px;
  font-style: italic;
}

.weather-image img {
  width: 100%;
}

</style>
