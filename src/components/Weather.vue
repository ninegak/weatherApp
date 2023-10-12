<script>
import axios from 'axios'
import DaysWeather from './DaysWeather.vue'
export default {
    name: "WeatherApp",
    components: {
        DaysWeather,
        axios
  },
  props: {
    city: String,
  },
  data() {
    return {
        cityname: this.city,
        temperature: null,
        description: null,
        iconUrl: null,
        date: null,
        time: null,
        humidity: null,
        sea_level: null,
        wind: null,
        country: null,
        monthNames: ["January","February","March","April","May","June","July", "August","September", "October", "November", "December"],
    }
  },
  async created() {
    const respone = await axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=aa50d4c6a393e82b9fc0e463acd1e4f9`)
    const weatherData = respone.data;
    this.temperature = weatherData.main.temp;
    this.description = weatherData.weather[0].description;
    this.name = weatherData.name;
    this.sea_level = weatherData.main.sea_level;
    this.humidity = weatherData.main.humidity;
    this.wind = weatherData.wind.speed;
    this.country = weatherData.sys.country;

    this.iconUrl = `https://api.openweathermap.org/img/w/${weatherData.weather[0].icon}.png`;
    const d = new Date();
    this.date = d.getDate() + '-' + this.monthNames[d.getMonth()] + '-' + d.getFullYear();
    this.time = d.getHours() + ':' + d.getMinutes() + ':' + d.getSeconds();
    console.log(weatherData);
  }
}
</script>

<template>
  <div class="container p-0">
    <div class="d-flex">
        <div class="card main-div w-100">
            <div class="p-3">
                <h2 class="mb-1 day">Today</h2>
                <p class="text date mb-0">{{date}}</p>
                <small>{{time}}</small>
                <h2 class="place"><i class="fa fa-location">{{ name }} <small>{{country}}</small></i></h2>
                <div class="temp">
                    <h1 class="wheather-temp">{{ temperature }}&deg;C</h1>
                    <h2 class="text">{{ description }} <img :src="iconUrl"> </h2>
                </div>
            </div>
        </div>
    
        <div class="card card-2 w-100">
        <table class="m-4">
            <tbody>
                <tr>
                    <th>Sea Level</th>
                    <td>{{ sea_level }}</td>
                </tr>
                <tr>
                    <th>Humidity</th>
                    <td>{{humidity}}</td>
                </tr>
                <tr>
                    <th>wind</th>
                    <td>{{ wind }}</td>
                </tr>
            </tbody>
        </table>

        <DaysWeather :cityname="cityname"/>

        <div id="div_Form" class="d-flex m-3 justify-content-center">
            <form action="">
                <input type="button" value="Change Location" class="btn change-btn btn-primary">
            </form>
        </div>
    </div>
    </div>

  </div>
</template>

<style scoped>


</style>
