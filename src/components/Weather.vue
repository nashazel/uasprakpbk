<template>
  <div class="centered-content">
    <div class="weather-widget">
      <h2 class="widget-title">Weather Forecast</h2>
      <q-input
        filled
        v-model="searchQuery"
        label="Enter City Name"
        class="location-input"
      />
      <q-btn @click="searchWeather" class="search-button">Search</q-btn>
      <div v-if="loading" class="loading-message">Loading data...</div>
      <div v-else-if="weatherData" class="weather-result">
        <div class="weather-info">
          <p class="city-name">{{ weatherData.name }}</p>
          <p class="temperature">{{ weatherData.main.temp }}°C</p>
          <img
            :src="getWeatherIconUrl(weatherData.weather[0].icon)"
            :alt="weatherData.weather[0].description"
            class="weather-icon"
          />
          <p class="weather-description">
            {{ weatherData.weather[0].description }}
          </p>
        </div>
        <div class="additional-info">
          <p>Humidity: {{ weatherData.main.humidity }}%</p>
          <p>Wind Speed: {{ weatherData.wind.speed }} m/s</p>
        </div>
      </div>
      <div v-else-if="error" class="error-message">{{ error }}</div>
    </div>
  </div>
</template>

<script>
import { ref } from "vue";
import axios from "axios";
import { QInput, QBtn } from "quasar";

export default {
  components: { QInput, QBtn },
  setup() {
    const searchQuery = ref("");
    const weatherData = ref(null);
    const loading = ref(false);
    const error = ref(null);

    const searchWeather = async () => {
      loading.value = true;
      error.value = null;

      try {
        const response = await axios.get(
          `https://api.openweathermap.org/data/2.5/weather?q=${searchQuery.value}&appid=bcbc6cc1860d02bd1f4306314c08d7e0&units=metric`
        );
        if (response.data.cod !== 200) {
          throw new Error("City not found");
        }
        weatherData.value = response.data;
      } catch (error) {
        console.error(error);
        error.value = "Failed to fetch weather data or city not found";
      } finally {
        loading.value = false;
      }
    };

    const getWeatherIconUrl = (iconCode) => {
      return `https://openweathermap.org/img/wn/${iconCode}.png`;
    };

    return {
      searchQuery,
      weatherData,
      loading,
      error,
      searchWeather,
      getWeatherIconUrl,
    };
  },
};
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@400;600;700&display=swap');

.centered-content {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
  background: linear-gradient(135deg, #f3a6b5 10%, #fdd0e5 100%);
  font-family: 'Poppins', sans-serif;
}

.weather-widget {
  max-width: 600px;
  width: 100%;
  padding: 30px;
  border-radius: 15px;
  background-color: rgba(223, 0, 156, 0.9);
  box-shadow: 0px 0px 20px 0px rgba(0, 0, 0, 0.1);
  text-align: center;
  backdrop-filter: blur(10px);
  animation: fadeIn 1s ease-in-out;
}

.widget-title {
  font-size: 2.5rem;
  margin-bottom: 20px;
  color: #333;
  font-weight: 700;
  letter-spacing: 1px;
}

.location-input {
  width: 100%;
  margin-bottom: 20px;
}

.search-button {
  width: 100%;
  padding: 12px 20px;
  background-color: #e91e63;
  color: #fff;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s ease, transform 0.3s ease;
  font-size: 1em;
  font-weight: 600;
}

.search-button:hover {
  background-color: #c2185b;
  transform: translateY(-2px);
}

.loading-message {
  font-style: italic;
  color: #888;
  margin-top: 10px;
}

.weather-result {
  margin-top: 20px;
  padding: 15px;
  border-radius: 10px;
  box-shadow: 0px 0px 15px rgba(0, 0, 0, 0.2);
  background-color: #ffffff;
  transition: box-shadow 0.3s ease, transform 0.3s ease;
}

.weather-result:hover {
  box-shadow: 0px 0px 25px rgba(0, 0, 0, 0.3);
  transform: translateY(-5px);
}

.weather-info {
  margin-bottom: 10px;
  text-align: center;
}

.city-name {
  font-weight: 600;
  color: #333;
  font-size: 1.75rem;
}

.temperature {
  font-size: 3.5rem;
  color: #e91e63;
}

.weather-icon {
  width: 100px;
  height: 100px;
  margin: 10px auto;
}

.weather-description {
  font-style: italic;
  color: #333;
}

.additional-info {
  margin-top: 10px;
  color: #555;
}

.additional-info p {
  margin: 5px 0;
}

.error-message {
  color: red;
  margin-top: 10px;
  text-align: center;
}

@keyframes fadeIn {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}
</style>
