<template>   
      <main :class="{ 'warm': isWarmWeather }">
      <div class="search-box">
        <input type="text" class="search-bar" placeholder="Search..." v-model="query"
        @keypress="fetchWeather" />        
      </div>
      <div class="weather-wrap" v-if="typeof weather.main != 'undefined' ">
        <div class="location-box">
          <div class="location">
            {{ weather.name }}, {{ weather.sys.country }}
          </div>
          <div class="date">{{ dateBuilder() }}</div>
        </div>

        <div class="weather-box"> 
          <div class="temp">{{ Math.round(weather.main.temp) }}°c</div>
          <div class="weather">{{ weather.weather[0].main }}</div>
        </div>
      </div>
    </main>
</template>

<script>
export default {
 name: 'WeatherWidget',

 data() {
  return {
    api_key: 'f1d44e1029400d937bbbbc4c372d95c7',
    api_base_url: 'https://api.openweathermap.org/data/2.5/',
    query: '',
    weather: {},
    temperature: null
  }
 },

 mounted() {
  console.log('API Key:', process.env.VUE_APP_API_KEY);
  },

 methods: {
    fetchWeather(event) {
      if (event.key == "Enter") {
        fetch(`${this.api_base_url}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
          .then(res => res.json())
          .then(data => {
            this.weather = data;
            this.temperature = data.main.temp; // Extract temperature from the API response
            this.clearQuery();
          })
          .catch(error => {
            console.error('Error fetching weather data:', error);
          });
      }
    },
    
    dateBuilder() {
        let d = new Date();
  
        let months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];
  
        let days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"];
  
        let day = days[d.getDay()];
        let date = d.getDate();
        let month = months[d.getMonth()];
        let year = d.getFullYear();
  
        return `${day} ${date} ${month} ${year}`;
    },

    clearQuery() {
      this.query = ''; // Reset the query to an empty string
    }
  },
  
  computed: {
    isWarmWeather() {
      return this.temperature && this.temperature > 16; // Check if temperature is greater than 16 degrees Celsius
    }
  }
}
</script>


<style scoped>
main {
  min-height: 100vh;
  padding: 25px;
  background-image: linear-gradient(to bottom, rgba(0,0,0,.25), rgba(0, 0, 0, .75));
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
  border: none;
  border-radius: 0 14px 0 14px;
  outline: none;
  background: none;
  box-shadow: 0 0 8px rgba(0, 0, 0, .25);
  background-color: rgba(255, 255, 255, .5);
  transition: .4s;
  text-transform: capitalize;
}

.search-box .search-bar:focus {
  background-color: rgba(255, 255, 255, .75);
  box-shadow: 0 0 16px rgba(0, 0, 0, .25);
  border-radius: 14px 0 14px 0;
}

.location-box .location {
  color: #fff;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, .25)
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

  text-shadow: 3px 6px rgba(0, 0, 0, .25);
  background-color:rgba(255, 255, 255, .25);
  border-radius: 16px;
  margin: 30px 0px;

  box-shadow: 3px 6px rgba(0, 0, 0, .25);
}

.weather-box .weather {
  color: #FFF;
  font-size: 48px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, .25);
}

.warm {
  background-image: url('../assets/warm-bg.jpg');
}
</style>
