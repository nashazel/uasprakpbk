<template>
  <div id="app">
    <Navbar />
    <div class="content">
      <router-view />
    </div>
  </div>
</template>

<script>
import Navbar from './components/Navbar.vue'

export default {
  name: 'App',
  components: {
    Navbar
  }
}
</script>

<style scoped>
/* Mengatur font dan latar belakang aplikasi */
#app {
  font-family: 'Poppins', sans-serif;
  text-align: center;
  padding: 30px;
  background: linear-gradient(135deg, #f5f7fa, #c3cfe2);
  min-height: 100vh;
}

/* Mengatur gaya judul aplikasi */
h1 {
  color: #333;
  margin-bottom: 20px;
  font-size: 2.5rem;
  font-weight: 700;
  letter-spacing: 1px;
}

/* Mengatur gaya konten halaman */
.content {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  margin-top: 20px;
  background: #fff;
  padding: 20px;
  border-radius: 15px;
  box-shadow: 0 0 20px rgba(0, 0, 0, 0.1);
  backdrop-filter: blur(10px);
  width: 80%;
  margin: auto;
}

/* Mengatur gaya tombol */
button {
  padding: 12px 20px;
  background-color: #007bff;
  color: #fff;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s, transform 0.3s;
  font-size: 1em;
  font-weight: 600;
}

button:hover {
  background-color: #0056b3;
  transform: translateY(-2px);
}

/* Mengatur gaya input */
input {
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 5px;
  margin-bottom: 10px;
  width: 100%;
  box-sizing: border-box;
}

/* Mengatur gaya pesan loading */
.loading {
  font-style: italic;
  color: #888;
}

/* Mengatur gaya konten cuaca */
.weather-info {
  margin-top: 30px;
  text-align: left;
}

.weather-info p {
  margin: 10px 0;
  font-size: 1.2em;
}

/* Tambahkan animasi fade-in untuk konten */
@keyframes fadeIn {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}

.content {
  animation: fadeIn 1s ease-in-out;
}
</style>
