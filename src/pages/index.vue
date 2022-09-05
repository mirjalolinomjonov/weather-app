<script setup>
</script>

<template>
  <main
    id="home"
    :class="
      typeof weather.main != 'undefined' &&
      (weather.main.temp > 16 && weather.main.temp < 22
        ? 'warm'
        : weather.main.temp >= 22
        ? 'clean'
        : '')
    "
  >
    <div class="main">
      <form @submit.prevent="fetchWeather" class="search-box">
        <input
          class="search-bar"
          type="text"
          placeholder="Search County"
          v-model="query"
        />
      </form>

      <div class="weather-wrap" v-if="typeof weather?.main != 'undefined'">
        <div class="location-box">
          <div class="location">
            {{ weather?.name }}, {{ weather?.sys?.country }}
          </div>
          <div class="date">{{ dateBuilder() }}</div>
        </div>
        <div class="weather-box">
          <div class="temp">{{ Math.round(weather?.main?.temp) }}°C</div>
          <figure>
            <img
              :src="`http://openweathermap.org/img/wn/${weather?.weather[0]?.icon}@4x.png`"
              alt=""
            />
            <figcaption class="weather">
              {{ weather?.weather[0]?.description }}
            </figcaption>
          </figure>
        </div>
      </div>
    </div>
  </main>
</template>

<script>
export default {
  name: "Home",
  data() {
    return {
      api_key: "7f8eb92dd49872de1b58298955efeacc",
      url_base: "https://api.openweathermap.org/data/2.5/",
      query: "Tashkent",
      weather: {},
    };
  },

  mounted() {
    this.fetchWeather();
  },

  methods: {
    fetchWeather() {
      fetch(
        `${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`
      )
        .then((res) => {
          return res.json();
        })
        .then(this.setResults);
    },

    setResults(results) {
      console.log(results);
      this.weather = results;
    },

    dateBuilder() {
      let d = new Date();
      let months = [
        "January",
        "February",
        "March",
        "April",
        "May",
        "June",
        "July",
        "August",
        "September",
        "October",
        "November",
        "December",
      ];
      let days = [
        "Sunday",
        "Monday",
        "Tuesday",
        "Wednesday",
        "Thursday",
        "Friday",
        "Saturday",
      ];
      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();
      return `${day} ${date} ${month} ${year}`;
    },
  },
};
</script>

<style lang="scss">
#home {
  background-image: url("@/static/cold.jpg");
  background-size: cover;
  background-position: center;
  transition: 0.4s;
}
#home.warm {
  background-image: url("@/static/warm.jpg");
}
#home.clean {
  background-image: url("@/static/clean.jpg");
}
.main {
  display: grid;
  grid-template-columns: 50% 50%;
  min-height: 100vh;
  padding: 25px;
  background: linear-gradient(
    to bottom,
    rgba(0, 0, 0, 0.25),
    rgba(0, 0, 0, 0.6)
  );
}
.search-box {
  width: 90%;
  .search-bar {
    margin-top: 30vh;
    display: block;
    width: 100%;
    padding: 15px;
    color: #353535;
    font-size: 20px;
    border: none;
    outline: none;
    appearance: none;
    text-transform: capitalize;
    background: rgba(255, 255, 255, 0.5);
    border-radius: 0 16px 0 16px;
    box-shadow: 0 0 8px rgba(0, 0, 0, 0.25);
    transition: 0.4s;
    &:focus {
      box-shadow: 0 0 16px rgba(0, 0, 0, 0.25);
      background-color: rgba(255, 255, 255, 0.75);
      border-radius: 16px 0 16px 0;
    }
  }
}
.location-box {
  .location {
    margin-top: 10vh;
    margin-bottom: 15px;
    color: #fff;
    font-size: 32px;
    font-weight: 500;
    text-align: center;
    text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
  }
  .date {
    color: #fff;
    font-size: 20px;
    font-weight: 300;
    font-style: italic;
    text-align: center;
  }
}
.weather-box {
  text-align: center;
  .temp {
    color: #fff;
    display: inline-block;
    padding: 10px 25px;
    font-size: 100px;
    font-weight: 900;
    text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
    background-color: rgba(255, 255, 255, 0.25);
    border-radius: 16px;
    margin: 30px 0;
    box-shadow: rgba(0, 0, 0, 0.25);
  }
  .weather {
    color: #fff;
    font-size: 28px;
    font-weight: 700;
    font-style: italic;
    text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  }
}
@media screen and (max-width: 576px) {
  .main {
    display: flex;
    flex-direction: column;
  }
  .search-box {
    .search-bar {
      margin-top: 10vh;
    }
  }
  .weather-box {
    .temp {
      font-size: 88px;
      font-weight: 500;
    }
  }
}
</style>