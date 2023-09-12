<template>
  <div id="app" :class="{ warm: weather && weather.main && weather.main.temp > 16 }">
    <p>Weather App</p>
    <main>
      <div class="search-box">
        <input
          type="text"
          class="search-bar"
          placeholder="Search..."
          v-model="query"
          @keypress.enter="fetchWeather"
        />

        <div class="weather-wrap" v-if="weather">
          <div class="location-box">
            {{ weather.name || 'Location Not Available' }},
            {{ weather.sys ? weather.sys.country || 'Country Not Available' : 'Country Not Available' }}
          </div>
          <div class="date">{{ dateBuilder() }}</div>
        </div>

        <div class="weather-box" v-if="weather && weather.main">
          <div class="temp">{{ weather.main.temp }}&deg;C</div>
          <div class="weather">{{ weather.weather[0].description }}</div>
          <div class="wind-speed">Wind Speed: {{ weather.wind ? weather.wind.speed + ' m/s' : 'Not Available' }}</div>
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
      api_key: '8f561951a18a38be63d9a0ff8308feca',
      url_base: 'https://api.openweathermap.org/data/2.5/',
      query: '',
      weather: null,
    };
  },
  methods: {
    async fetchWeather() {
      try {
        const response = await fetch(
          `${this.url_base}weather?q=${this.query}&units=metric&appid=${this.api_key}`
        );
        const data = await response.json();
        this.weather = data;
        console.log(data);
      } catch (error) {
        console.error('Error fetching weather data:', error);
      }
    },
    dateBuilder() {
      const d = new Date();
      const months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];
      const days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"];

      const day = days[d.getDay()];
      const date = d.getDate();
      const month = months[d.getMonth()];
      const year = d.getFullYear();

      return `${day}, ${date} ${month}, ${year}.`;
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

body {
  display: flex;
  justify-content: center;
  font-family: 'Montserrat', sans-serif;
  width: 100%;
  height: 100vh;
  background: #202C36;
}

#app {
  display: flex;
  flex-direction: column;
  align-items: center;
  transition: 0.4s;
  width: 100%;
}

p{
  color: #fff;
  font-size: 55px;
  font-weight: 800;
  letter-spacing: 3px;
  text-decoration: underline;
  text-align: center;
  text-shadow: 1px 3px rgba(200, 192, 192, 0.481);
  margin: 55px auto;
}

main {
  border-radius: 16px;
  display: flex;
  justify-content: center;
  padding: 25px;
  width: 800px;
  background-image: linear-gradient(to right, rgba(107, 104, 104, 0.5), rgba(0, 0, 0, 0.75));
}

.search-box {
  width: 100%;
  margin-bottom: 30px;
}

.search-bar {
  width: 100%;
}

.search-box .search-bar {
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
  border-radius: 0px 16px 0px 16px;
  transition: 0.4s;

}

.search-box .search-bar:focus {
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.75);
  border-radius: 16px 0px 16px 0px;
}

.location-box {
  margin-top: 25px;
  color: #fff;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}

.date {
  margin-top: 10px;
  color: #fff;
  font-size: 20px;
  font-weight: 300;
  font-style: italic;
  text-align: center;
}

.weather-box{
  text-align: center;
}

.weather-box .temp {
  display: inline-block;
  padding: 10px 25px;
  color: #fff;
  font-size: 102px;
  font-weight: 900;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.25);
  border-radius: 16px;
  margin: 30px 0;
  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

.weather-box .weather  {
  color: #fff;
  font-size: 48px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

.wind-speed{
  color: #fff;
  font-size: 48px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
</style>
