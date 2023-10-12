<script>
import Weather from './components/Weather.vue';
export default {
  name: "App",
  components: {
    Weather
  },
  data() {
    return {
      city: '',
      showWeather:false,
    }
  },
  methods: {
    async searchWeather() {
      this.showWeather = false;
      await this.$nextTick();
      this.showWeather = true;
    }
  }
}
</script>

<template>
  <div class="header container p5">
    <h1 class="mb-5">Weather App</h1>
    <div class="d-flex justify-content-center h-100">
      <div class="searchbar w-50 mx-2">
        <input type="text" class="input form-contral" v-model="city" placeholder="search some place">
        <button class="btn-search btn btn-primary h-10 w-10" @click="searchWeather">Search <i class="fa fa-search"></i> </button>
      </div>
      
    </div>
    <br>
    <Weather :city="city" v-if="showWeather"/>
  </div>
</template>

<style scoped>


</style>
