<template>
  <div id="app">
    <div class="container">
      <h1>Previsão do Tempo</h1>
      <div class="search">
        <input v-model="cidade" @keyup.enter="fetchWeather" placeholder="Digite o nome de uma cidade" />
        <button @click="fetchWeather">Buscar</button>
      </div>
      <div v-if="weather" class="weather-card">
        <h2>{{ weather.nome }}</h2>
        <p class="temp">{{ weather.main.temp }} °C</p>
        <p class="description">{{ weather.weather[0].description }}</p>
        <p>Umidade: {{ weather.main.humidity }}%</p>
      </div>
      <p v-if="erro" class="erro">{{ erro }}</p>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  cidade: 'App',
  data() {
    return {
      cidade: '',
      weather: null,
      erro: null,
    };
  },
  methods: {
    async fetchWeather() {
      if (!this.cidade) {
        this.erro = 'Por favor, insira o nome de uma cidade.';
        return;
      }

      this.erro = null;
      try {
        const apiKey = 'ceb071a5413e1004c2342ba9624cd5a8';
        const response = await axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${this.cidade}&appid=${apiKey}&units=metric&lang=pt_br`);
        this.weather = response.data;
      } catch (err) {
        this.erro = 'Cidade não encontrada !! Tente novamente.';
        this.weather = null;
      }
    },
  },
};
</script>

<style scoped>
#app {
  font-family: 'Open Sans', sans-serif;
  color: #000000;
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  background-color: #8b8d8d; 
}

.container {
  text-align: center;
  background: rgba(0, 153, 255, 0.363); 
  border-radius: 30px;
  box-shadow: 0 16px 16px rgba(0, 0, 0, 0.2);
  padding: 30px;
  max-width: 600px;
  width: 100%;
}

h1 {
  margin-bottom: 20px;
  color: #003366; 
  font-size: 2.5em;
  text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.5);
}

.search {
  margin-bottom: 20px;
}

input {
  padding: 12px;
  border: 2px solid #002d5a; 
  border-radius: 6px;
  outline: none;
  transition: border-color 0.3s ease, box-shadow 0.3s ease;
  width: calc(100% - 110px);
  margin-right: 10px;
  font-size: 1em;
}

input:focus {
  border-color: #004080; 
  box-shadow: 0 0 8px rgba(0, 0, 128, 0.5);
}

button {
  padding: 12px;
  border: none;
  border-radius: 6px;
  background-color: #004080; 
  color: #fff;
  cursor: pointer;
  transition: background-color 0.3s ease, transform 0.3s ease;
  font-size: 1em;
}

button:hover {
  background-color: #003366; 
  transform: scale(1.05);
}

.weather-card {
  margin-top: 20px;
  padding: 20px;
  border-radius: 12px;
  background: rgba(192, 192, 192, 0.8); 
  box-shadow: 0 6px 12px rgba(0, 0, 0, 0.2);
}

.weather-card h2 {
  margin: 0;
  color: #003366; 
  font-size: 2em;
}

.temp {
  font-size: 3em;
  margin: 10px 0;
  color: #004080; 
}

.description {
  font-style: normal;
  color: #333;
}

.erro {
  color: #ff2b2b; 
  margin-top: 20px;
  font-weight: bold;
}
</style>
