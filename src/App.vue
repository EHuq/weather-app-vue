<template>
  <main
    class="backgroundApp"
    :class="typeof weather.main != 'undefined' && weather.main.temp < 15 ? 'cold': ''"
  >
    <div>
      <div class="searchbar-container">
        <input
          v-model="searchTerm"
          type="text"
          class="searchbar"
          placeholder="Search..."
          @keypress="fetchWeather"
        />
      </div>
      <div class="error-wrap" v-if="error">
        <div class="error">Cannot Find City Sorry!</div>
      </div>
      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location">{{weather.name}}, {{weather.sys.country}}</div>
          <div class="date">{{dateBuilder()}}</div>
        </div>
        <div class="weather-box">
          <div class="temperature">{{weather.main.temp.toFixed(1)}}°C</div>
          <div class="weather">{{weather.weather[0].main}}</div>
        </div>
      </div>
    </div>
  </main>
</template>

<script>
export default {
  data() {
    return {
      error: false,
      apiKey: "6798adcef54e41406b45c385ba373990",
      baseURL: "https://api.openweathermap.org/data/2.5/",
      searchTerm: "",
      weather: {}
    };
  },
  methods: {
    fetchWeather(e) {
      if (e.key == "Enter") {
        fetch(
          `${this.baseURL}weather?q=${this.searchTerm}&units=metric&APPID=${this.apiKey}`
        )
          .then(res => {
            return res.json();
          })
          .then(this.setData);
      }
    },
    setData(data) {
      this.weather = data;
      if (this.weather.message === "city not found") {
        this.error = true;
      } else {
        this.error = false;
      }
    },
    dateBuilder() {
      let d = new Date();
      let months = [
        "Jan.",
        "Feb.",
        "March",
        "April",
        "May",
        "June",
        "July",
        "Aug.",
        "Sep.",
        "Oct.",
        "Nov.",
        "Dec."
      ];
      let days = [
        "Saturday",
        "Sunday",
        "Monday",
        "Tuesday",
        "Wednesday",
        "Thursday",
        "Friday"
      ];

      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();

      return `${day}, ${month} ${date}, ${year}`;
    }
  }
};
</script>

<style>
@import url("https://fonts.googleapis.com/css?family=Lato");

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: "Lato", sans-serif;
}

/* .backgroundApp  */

.backgroundApp {
  background-image: linear-gradient(
      to bottom,
      rgba(0, 0, 0, 0),
      rgba(0, 0, 0, 0.3)
    ),
    url("./assets/images/warm-bg.jpg");
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}

main {
  min-height: 100vh;
  padding: 25px;
}

.cold {
  background-image: linear-gradient(
      to bottom,
      rgba(0, 0, 0, 0),
      rgba(0, 0, 0, 0.3)
    ),
    url("./assets/images/cold-bg.jpg");
}

.searchbar-container {
  width: 100%;
  margin-bottom: 30px;
}

.searchbar-container .searchbar {
  display: block;
  width: 100%;
  padding: 15px;
  color: #313131;
  font-size: 20px;
  appearance: none;
  border: none;
  background: none;
  box-shadow: 0 0 15px rgba(0, 0, 0, 0.5);
  background-color: rgba(255, 255, 255, 0.5);
  border-radius: 5px;
  transition: 0.4s;
}

.searchbar-container .searchbar:focus {
  outline: none;
  border-radius: 30px;
  background-color: rgba(255, 255, 255, 0.7);
  box-shadow: 0 0 25px rgba(0, 0, 0, 1);
}

.weather-wrap {
  max-width: 400px;
  margin: auto;
}

.location-box .location {
  color: white;
  font-size: 32px;
  font-weight: 700;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.4);
}

.location-box .date {
  color: white;
  font-size: 20px;
  font-weight: 300;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.4);
  /* font-style: italic; */
}

.weather-box {
  text-align: center;
}

.weather-box .temperature {
  display: inline-block;
  padding: 10px 25px;
  color: #ffffff;
  font-size: 70px;
  font-weight: 900;

  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 200, 200, 0.25);
  border-radius: 16px;
  margin: 30px 0px;
  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

.weather-box .weather {
  color: #ffffff;
  font-size: 48px;
  font-weight: 800;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

.error-wrap {
  text-align: center;
}

.error {
  display: inline-block;
  padding: 10px 25px;
  color: #ffffff;
  font-size: 50px;
  font-weight: 900;

  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  margin: 30px 0px;
}
</style>
