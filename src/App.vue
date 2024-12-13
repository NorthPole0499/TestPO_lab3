<script>
import axios from 'axios'

export default {
  data() {
    return {
      city: "",
      error: "",
      info: null
    }
  },
  computed: {
    cityName() {
      return "«" + this.city + "»";
    },
    showTemp() {
      return "Температура: " + this.info.main.temp;
    },
    showFeelsLike() {
      return "Ощущается как: " + this.info.main.feels_like;
    },
    showMinTemp() {
      return "Минимальная температура: " + this.info.main.temp_min;
    },
    showMaxTemp() {
      return "Максимальная температура: " + this.info.main.temp_max;
    }

  },
  methods: {
    getWeather() {
      if (this.city.trim().length < 2) {
        this.error = "Необходимо более 1 символа.";
        return false;
      }
      this.error = "";

      axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=75d67aac64099d483b0fdbc6d3a55a92`)
      .then(res => (this.info = res.data));
    }
  }
}
</script>

<template>
  <div class="wrapper">
    <h1>Погодное отображение</h1>
    <p id="city-label">Узнать погоду в {{ city == "" ? "в вашем городе" : cityName }}</p>
    <input id="input-city" type="text" v-model="city" placeholder="Введите город">
    <button id="get-button" v-if="city!=''" @click="getWeather()">Получить погоду</button>
    <button id="no-button" disabled v-else>Нет города</button>
    <p id="error-input" class="error">{{ error }}</p>

    <div v-if="info != null">
      <p id="temp">{{ showTemp }}</p>
      <p id="feelsLike">{{ showFeelsLike }}</p>
      <p id="minTemp">{{ showMinTemp }}</p>
      <p id="maxTemp">{{ showMaxTemp }}</p>
    </div>
  </div>
</template>

<style scoped>

.error {
  color: #d03939;
}

.wrapper {
  width: 900px;
  height: 500px;
  border-radius: 50px;
  background: #1f0f24;
  padding: 20px;
  text-align: center;
  color: #fff;
}

.wrapper h1 {
  margin-top: 50px;
}

.wrapper p {
  margin-top: 20px;
}

.wrapper input {
  margin-top: 30px;
  border: 0%;
  background: transparent;
  border-bottom: 2px solid #110813;
  color: #fcfcfc;
  font-size: 14px;
  padding: 5px 8px;
  outline: none;
}

.wrapper input:focus {
  border-bottom-color: #6e2d7d;
}

.wrapper button:disabled {
  background: #a49e8a;
  cursor: not-allowed;
}

.wrapper button {
  background: #e3bc4b;
  color: #fff;
  border-radius: 10px;
  border: 2px solid #b99935;
  padding: 10px 15px;
  margin-left: 20px;
  cursor: pointer;
  transition: transform 500ms ease;
}

.wrapper button:hover {
  transform: scale(1.1) translateY(-5px)
}
</style>
