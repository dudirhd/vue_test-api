<template>
  <div className="wrapper">
    <h1>Погодное приложение</h1>
    <p>Узнать погоду в {{ city == "" ? "вашем городе" : cityName}}</p>
    <input type="text" v-model="city" placeholder="Введите город">
    <button v-if="city != ''" @click="getWeather()">Получить погоду</button>
    <button disabled v-else>Введите название города</button>
    <p className="error">{{ error }}</p>

    <div v-if="info != null" className="info">
      <p >{{ showTemp }}</p>
      <p >{{ showFeelsLike }}</p>
      <p >{{ showMinTemp }}</p>
      <p >{{ showMaxTemp }}</p>
    </div>

  </div>
</template>


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
      return "<" + this.city + ">"
    },
    showTemp() {
      return "Темпетура: " + this.info.data.main.temp
    },
    showFeelsLike() {
      return "Ощущается как: " + this.info.data.main.feels_like
    },
    showMinTemp() {
      return "Минимальна темпетура: " + this.info.data.main.temp_min
    },
    showMaxTemp() {
      return "Максимальная темпетура: " + this.info.data.main.temp_max
    }
  },
  methods: {
    getWeather() {
      if(this.city.trim().length < 2) {
        this.error = "Нужно название более одного символа("
        return false
      }

      this.error = "";

      axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=11d656a57c74b3e39c468f72d3479af7`)
        .then(res => (this.info = res)) 
    }
  }
}
</script>

<style scoped>
.wrapper {
  width: 900px;
  height: 500px;
  border-radius: 50px;
  padding: 20px;
  background: #1f0f24;
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
  background-color: transparent;
  border: 0;
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
  background: #746027;
  cursor: not-allowed;
}

.wrapper button {
  background-color: #e3bc4d;
  color: #fff;
  border-radius: 10px;
  border: 2px solid #b99935;
  padding: 10px 15px;
  margin-left: 20px;
  cursor: pointer;
  transition: transform 500ms ease;
}

.wrapper button:hover {
  transform: scale(1.1) translateY(-5px);
}

.error {
  color: #d03939;
}
</style>
