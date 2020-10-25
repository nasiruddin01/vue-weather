<template>
  <div id="app">
      <main :class="{ 'warm' : isWarm }">
      <h2>Weather App</h2>
      <div class="search-box">
        <input
          v-model="query"
          @keypress="fetchWeather"
          type="text"
          class="search-bar"
          placeholder="Search...."
        />
      </div>
      <div class="weather-wrap" v-if="typeof weather.main !='undefined'">
        <div class="location-box">
          <div class="location">{{ weather.name }},{{weather.sys.country}}</div>
          <div class="date">{{ dateBuilder() }}</div>
        </div>
        <div class="weather-box">
          <div class="temp">{{Math.round(weather.main.temp)}}°c</div>
          <div class="weather">{{weather.weather[0].main}}</div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
export default {
  name: "app",
  data() {
    return {
      api_key: "f064a082d05edbbddf41d7254c8176ed",
      url_base: "https://api.openweathermap.org/data/2.5/",
      query: "",
      weather: {},
    };
  },
  computed: {
    isWarm() {
      return this.weather.main != undefined && this.weather.main.temp > 16
        ? true
        : false;
    },
  },
  watch: {
    isWarm(value) {
          },
  },
  methods: {
    fetchWeather(e) {
      if (e.key == "Enter") {
        fetch(
          `${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`
        )
          .then((res) => {
            return res.json();
          })
          .then(this.setResults);
               }
    },
    setResults(results) {
      this.weather = results;
      console.log(results);
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
@import url("https://fonts.googleapis.com/css2?family=Montserrat:ital,wght@0,100;0,200;0,300;0,400;0,500;0,600;0,700;1,100;1,200;1,300;1,400;1,500;1,600;1,700&display=swap");
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

#app {
  font-family: "Montserrat", sans-serif;

  main {
    background-image: url("assets/cold-bg1.jpeg");
    background-size: cover;
    background-position: bottom;
    transition: 0.4s;
    min-height: 100vh;
    padding: 25px;
    background-image: linear-gradient(
      to bottom,
      rgba(0, 0, 0, 0.25),
      rgba(0, 0, 0, 0.75)
    );
     h1{
text-align: center;
margin: 1rem auto;
}
    .search-box {
      width: 100%;
      margin-bottom: 30px;
      .search-bar {
        display: block;
        width: 100%;
        padding: 15px;
        color: #313131;
        font-size: 20px;
        appearance: none;
        border: none;
        outline: none;
        background: none;
        box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
        background-color: rgba(255, 255, 255, 0.5);
        border-radius: 0px 16px 0px 16px;
        transition: 0.4s;
      }
      :focus {
        box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
        background-color: rgba(0, 0, 0, 0.75);
        border-radius: 16px 0px 16px 0px;
        color: #fff;
      }
     
    }
    .weather-wrap {
      .location-box {
        .location {
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
          display: inline-block;
          padding: 10px 25px;
          color: #fff;
          font-weight: 900;
          font-size: 102px;
          text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
          background-color: rgba(255, 255, 255, 0.5);
          border-radius: 16px;
          margin: 30px 0px;
          box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
        }
        .weather {
          color: #fff;
          font-size: 48px;
          font-weight: 700px;
          font-style: italic;
          text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
        }
      }
    }
  }
  .warm {
    background-image: url("assets/warm-bg.jpg");
  }
}
</style>
