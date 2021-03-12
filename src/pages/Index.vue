<template>
  <q-page class="bg-pink-800 text-gray-300 px-8 pt-3" :class="bgClass">

    <div class="search-box">
      <q-input
        v-model="search"
        @keyup.enter="getWeatherBySearch"
        placeholder="Enter city or country name..."
        borderless
        dark
      >
        <template v-slot:before>
          <q-icon @click="getLocation" name="location_on" />
        </template>

        <template v-slot:append>
          <q-btn @click="getWeatherBySearch" round dense flat icon="search" />
        </template>
      </q-input>
    </div>


    <template v-if="weatherData">
      <div class="text-center mt-52">
        <div class="text-4xl">
          {{ weatherData.name }}, {{ weatherData.sys.country }}
        </div>
        <div class="text-2xl text-gray-400 my-2">
          {{ weatherData.weather[0].main }},
          {{ weatherData.weather[0].description }}
        </div>
        <div class="text-6xl text-gray-400">
          {{ Math.round(weatherData.main.temp) }}&deg; C
        </div>
        <div class="img-icon">
          <img
            :src="`http://openweathermap.org/img/wn/${weatherData.weather[0].icon}@2x.png`"
            alt="weather-icon"
            class="mx-auto"
          />
        </div>
      </div>
    </template>

    <template v-else>
      <div class="text-center mt-52">
        <div class="text-5xl">Today's Weather</div>

        <div class="location-btn mt-8">
          <q-btn @click="getLocation" class="col btn" flat>
            <q-icon left size="3em" name="location_on" />
            <div>Find my location</div>
          </q-btn>
        </div>
      </div>
    </template>

    <div class="skyline">
      <img src="../assets/media/skyline.png" alt="nhnhn" />
    </div>
    
  </q-page>
</template>



<script>
export default {
  name: "PageIndex",
  data() {
    return {
      search: "",
      weatherData: null,
      lat: null,
      lon: null,
      apiUrl: "http://api.openweathermap.org/data/2.5/weather",
      apiKey: "5edcdf4946c86c9b6f9bc51109f006b7",
    };
  },

  computed: {
    bgClass() {
      if (this.weatherData) {
        if (this.weatherData.weather[0].icon.endsWith("n")) {
          return "bg-night";
        } else {
          return "bg-day";
        }
      }
    },
  },

  methods: {
    getLocation() {
      this.$q.loading.show();
      //for MAC
      if (this.$q.platform.is.electron) {
        this.$axios.get("https://freegeoip.app/json/").then((response) => {
          this.lat = response.data.latitude;
          this.lon = response.data.longitude;
          this.getWeatherByCoords();
        });
      } else {
        //Others
        navigator.geolocation.getCurrentPosition((position) => {
          console.log("position: ", position);
          this.lat = position.coord.latitude;
          this.lon = position.coord.longitude;
          this.getWeatherByCoords();
        });
      }
    },
    getWeatherByCoords() {
      this.$q.loading.show();
      //"http://api.openweathermap.org/data/2.5/weather?lat=24.893947&lon=91.865616&appid=5edcdf4946c86c9b6f9bc51109f006b7"
      this.$axios(
        `${this.apiUrl}?lat=${this.lat}&lon=${this.lon}&appid=${this.apiKey}&units=metric`
      ).then((response) => {
        // console.log("response:", response);
        this.weatherData = response.data;
        this.$q.loading.hide();
      });
    },
    getWeatherBySearch() {
      this.$q.loading.show();
      this.$axios(
        `${this.apiUrl}?q=${this.search}&appid=${this.apiKey}&units=metric`
      ).then((response) => {
        this.weatherData = response.data;
        this.$q.loading.hide();
      });
    },
  },
};
</script>


<style lang="stylus">
.bg-night {
  background: rgb(31, 41, 55);
}

.bg-day {
  background: teal;
}

.btn {
  width: 100%;
  padding: 10px 0;
  background: rgba(0, 0, 0, 0.2);
}
</style>