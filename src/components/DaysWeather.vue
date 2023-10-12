<template>
    <div class="days-tab text-center">
        <div v-if="loading" class="loading">Loading...</div>
        <ul v-elseclass="p-0">
            <li v-for="day in forecast" :key="day.date" class="li_active">
                <div class="py-3"><img :src="day.iconUrl"></div>
                <div class="py-3">{{getDayNam(day.date)}}</div>
                <div class="py-3">{{day.temperature}}&deg;C</div>
            </li>
        </ul>
    </div>
</template>

<script>
import axios from 'axios'
import moment from 'moment'
export default {
    props: {
        cityname: String,
    },
    data() {
        return {
            forecast: [],
            loading: true,
            iconUrl: null,
        }
    },
    mounted() {
        this.fetchWeatherData();
    },
    methods: {
        async fetchWeatherData() {
            const apiKey = 'aa50d4c6a393e82b9fc0e463acd1e4f9';
            const city = this.cityname;
            const apiUrl = `https://api.openweathermap.org/data/2.5/forecast?q=${city}&units=metric&appid=${apiKey}`

            await axios.get(apiUrl).then(Response => {
                const forecastData = Response.data.list;
                const filterdData = forecastData.map(item => {
                    return {
                        date: moment(item.dt_txt.split(' ')[0]),
                        temperature: Math.round(item.main.temp),
                        description: item.weather[0].description,
                        iconUrl: `https://api.openweathermap.org/img/w/${item.weather[0].icon}.png`,
                    }
                }).reduce((acc, item) => {
                    if(!acc.some(day => day.date.isSame(item.date, 'day')))
                    {
                        acc.push(item);
                    }
                    return acc;
                }, []).slice(1, 5);
                console.log(filterdData, "working");
                this.forecast = filterdData;
                this.loading = false;

            }).catch(error => {
                console.error('Error fetching weather data', error);
                this.loading = false;
            });
        },
        getDayNam(date) {
            return date.format('ddd');
        }
    }
}
</script>