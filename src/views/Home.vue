<template>
    <div class="centered-content">
      <div class="weather-widget">
        <div class="widget-header">
          <h2 class="widget-title">Informasi Cuaca</h2>
        </div>
        <q-input
          filled
          v-model="searchQuery"
          label="Masukkan Nama Kota"
          class="location-input"
          color="white"
        />
        <q-btn @click="searchWeather" class="search-button">Cari</q-btn>
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
            <p>Kelembaban: {{ weatherData.main.humidity }}%</p>
            <p>Kecepatan Angin: {{ weatherData.wind.speed }} m/s</p>
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
          error.value = "Gagal mengambil data cuaca atau kota tidak ditemukan";
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
  @import url('https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap');
  
  body {
    font-family: 'Roboto', sans-serif;
    background-color: #fafafa;
    color: #ff008c;
  }
  
  .centered-content {
    display: flex;
    justify-content: center;
    align-items: center;
    min-height: calc(100vh - 200px); /* Mengurangi tinggi navbar dari 100vh */
    padding: 20px;
  }
  
  .weather-widget {
    width: 100%;
    max-width: 400px;
    padding: 20px;
    border-radius: 15px;
    background-color: rgba(244, 5, 140, 0.9);
    box-shadow: 0 10px 30 rgba(255, 255, 255, 0.9);
    backdrop-filter: blur(10px);
    text-align: center;
    transition: transform 0.3s ease, box-shadow 0.3s ease;
  }
  
  .weather-widget:hover {
    transform: translateY(-5px);
    box-shadow: 0 15px 40px rgba(255, 255, 255, 0.9);
  }
  
  .widget-header {
    text-align: center;
    margin-bottom: 15px;
  }
  
  .widget-title {
    font-size: 2rem;
    color: #ffffff; /* Ubah warna menjadi hitam */
    margin: 0;
  }
  
  .location-input {
    width: 100%;
    margin-bottom: 15px;
  }
  
  .location-input .q-field__control {
    background-color: #ffffff !important;
  }
  
  .location-input .q-field__native {
    color: #ffffff !important;
  }
  
  .search-button {
    width: 100%;
    padding: 10px 0;
    background-color: #ff00ae;
    color: #f9f9f9;
    border: none;
    border-radius: 5px;
    cursor: pointer;
    transition: background-color 0.3s ease;
  }
  
  .search-button:hover {
    background-color: #ffffff;
  }
  
  .loading-message {
    font-style: italic;
    color: #ffffff;
    margin-top: 10px;
  }
  
  .weather-result {
    margin-top: 20px;
    padding: 15px;
    border-radius: 10px;
    background-color: rgba(255, 0, 208, 0.8);
    box-shadow: 0 5px 15px rgba(255, 255, 255, 0.9);
    transition: box-shadow 0.3s ease, transform 0.3s ease;
  }
  
  .weather-result:hover {
    box-shadow: 0 10px 25px rgba(255, 255, 255, 0.9);
    transform: translateY(-5px);
  }
  
  .weather-info {
    margin-bottom: 10px;
    text-align: center;
  }
  
  .city-name {
    font-weight: bold;
    color: #ffffff;
    font-size: 1.5rem;
  }
  
  .temperature {
    font-size: 3rem;
    color: #ffffff;
  }
  
  .weather-icon {
    width: 80px;
    height: 80px;
    margin: 10px auto;
  }
  
  .weather-description {
    font-style: italic;
    color: #ffffff;
  }
  
  .additional-info {
    margin-top: 10px;
    color: #ffffff;
  }
  
  .additional-info p {
    margin: 5px 0;
  }
  
  .error-message {
    color: rgb(255, 255, 255);
    margin-top: 10px;
    text-align: center;
  }
  </style>
  