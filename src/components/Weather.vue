<template>
  <div>
    <section v-if="errored">
      <p>
        К сожалению, мы не можем загрузить информацию о погоде. Попробуйте ещё
        раз
      </p>
    </section>

    <section v-else>
      <div v-if="loading">Загрузка...</div>
      <div v-else>
        <div class="container">
          <div class="weather-side">
            <div class="weather-gradient"></div>
            <div class="date-container">
              <h2 class="date-dayname"></h2>
              <span class="date-day">{{ time }}</span
              ><i class="location-icon" data-feather="map-pin"></i
              ><span class="location">Тула, Россия</span>
            </div>
            <div class="weather-container">
              <i class="weather-icon" data-feather="sun"></i>
              <h1 class="weather-temp">{{ info.instant.details.air_temperature }} °C</h1>
              <h3 class="weather-desc">
                {{ info.instant.details.cloud_area_fraction }} % облачность
              </h3>
            </div>
          </div>
          <div class="info-side">
            <div class="today-info-container">
              <div class="today-info">
                <div class="precipitation">
                  <span class="title">Атм. давление</span
                  ><span class="value"
                    >{{ info.instant.details.air_pressure_at_sea_level }} Па</span
                  >
                  <div class="clear"></div>
                </div>
                <div class="humidity">
                  <span class="title">Влажность</span
                  ><span class="value">{{ info.instant.details.relative_humidity }} %</span>
                  <div class="clear"></div>
                </div>
                <div class="wind">
                  <span class="title">Скорость ветра</span
                  ><span class="value">{{ info.instant.details.wind_speed }} м/с</span>
                  <div class="clear"></div>
                </div>
              </div>
            </div>
            <div class="week-container">
              <ul class="week-list">
                <li class="active">
                  <i class="day-icon" data-feather="sun"></i
                  ><span class="day-name">Вск</span
                  ><span class="day-temp">1.5 °C</span>
                </li>
                <li>
                  <i class="day-icon" data-feather="cloud"></i
                  ><span class="day-name">Пон</span
                  ><span class="day-temp">2 °C</span>
                </li>
                <li>
                  <i class="day-icon" data-feather="cloud-snow"></i
                  ><span class="day-name">Втр</span
                  ><span class="day-temp">1.2°C</span>
                </li>
                <li>
                  <i class="day-icon" data-feather="cloud-rain"></i
                  ><span class="day-name">Срд</span
                  ><span class="day-temp">1 °C</span>
                </li>
                <div class="clear"></div>
              </ul>
            </div>
            <div class="location-container">
              <button
                class="location-button"
                onClick="location.href=location.href"
              >
                <i data-feather="map-pin"></i><span>Обновить</span>
              </button>
            </div>
          </div>
        </div>
      </div>
    </section>
  </div>
</template>

<script>
import axios from "axios";
//import VueMoment from 'vue-moment'
import moment from "moment-timezone";
// 0-100
// 0-25 ясно
// 26-50 переменная_облачность
// 51-75 облачно
// 76-100 пасмурно
export default {
  name: "Weather",
  data() {
    return {
      info: {},
      time: "",
      tempDays: [],
      loading: true,
      errored: false,
      day: 0,
    };
  },
  filters: {
    formatDate: function (value) {
      return value;
    },
  },
  mounted() {
    if (this.day == 0) {
      axios
        .get(
          "https://api.met.no/weatherapi/locationforecast/2.0/compact?lat=54.12&lon=37.37"
        )
        .then(
          (response) =>
            (this.info =
              response.data.properties.timeseries[0].data)
          //(this.time = response.data.properties.meta.updated_at)
        )
        .catch(() => {
          this.errored = true;
        })
        .finally(() => (this.loading = false));
    }

    var date = moment(new Date()).format("DD/MM");
    this.time = date;
  },
};
// https://api.met.no/weatherapi
// https://corporate.foreca.com/en/api-point-forecasts?hsLang=en
</script>

<style>
a {
  text-decoration: none; /* Убираем подчёркивание */
}
p {
  color: white;
}
@import url("https://fonts.googleapis.com/css?family=Montserrat:400,700,900&display=swap");

:root {
  --gradient: linear-gradient(135deg, #72edf2 10%, #5151e5 100%);
}

* {
  -webkit-box-sizing: border-box;
  box-sizing: border-box;
  line-height: 1.25em;
}

.clear {
  clear: both;
}

body {
  margin: 0;
  width: 100%;
  height: 100vh;
  font-family: "Montserrat", sans-serif;
  background-color: #343d4b;
  display: -webkit-box;
  display: -ms-flexbox;
  display: flex;
  -webkit-box-align: center;
  -ms-flex-align: center;
  align-items: center;
  -webkit-box-pack: center;
  -ms-flex-pack: center;
  justify-content: center;
}

.container {
  border-radius: 25px;
  -webkit-box-shadow: 0 0 70px -10px rgba(0, 0, 0, 0.2);
  box-shadow: 0 0 70px -10px rgba(0, 0, 0, 0.2);
  background-color: #222831;
  color: #ffffff;
  height: 400px;
}

.weather-side {
  position: relative;
  height: 100%;
  border-radius: 25px;
  /* background-image: url("https://images.unsplash.com/photo-1559963110-71b394e7494d?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=675&q=80"); */
  background-image: url("https://img.geliophoto.com/tula/01_tula.jpg");
  width: 300px;
  -webkit-box-shadow: 0 0 20px -10px rgba(0, 0, 0, 0.2);
  box-shadow: 0 0 20px -10px rgba(0, 0, 0, 0.2);
  -webkit-transition: -webkit-transform 300ms ease;
  transition: -webkit-transform 300ms ease;
  -o-transition: transform 300ms ease;
  transition: transform 300ms ease;
  transition: transform 300ms ease, -webkit-transform 300ms ease;
  -webkit-transform: translateZ(0) scale(1.02) perspective(1000px);
  transform: translateZ(0) scale(1.02) perspective(1000px);
  float: left;
}

.weather-side:hover {
  -webkit-transform: scale(1.1) perspective(1500px) rotateY(10deg);
  transform: scale(1.1) perspective(1500px) rotateY(10deg);
}

.weather-gradient {
  position: absolute;
  width: 100%;
  height: 100%;
  top: 0;
  left: 0;
  background-image: var(--gradient);
  border-radius: 25px;
  opacity: 0.8;
}

.date-container {
  position: absolute;
  top: 25px;
  left: 25px;
}

.date-dayname {
  margin: 0;
}

.date-day {
  display: block;
}

.location {
  display: inline-block;
  margin-top: 10px;
}

.location-icon {
  display: inline-block;
  height: 0.8em;
  width: auto;
  margin-right: 5px;
}

.weather-container {
  position: absolute;
  bottom: 25px;
  left: 25px;
}

.weather-icon.feather {
  height: 60px;
  width: auto;
}

.weather-temp {
  margin: 0;
  font-weight: 700;
  font-size: 4em;
}

.weather-desc {
  margin: 0;
}

.info-side {
  position: relative;
  float: left;
  height: 100%;
  padding-top: 25px;
}

.today-info {
  padding: 15px;
  margin: 0 25px 25px 25px;
  /* 	box-shadow: 0 0 50px -5px rgba(0, 0, 0, 0.25); */
  border-radius: 10px;
}

.today-info > div:not(:last-child) {
  margin: 0 0 10px 0;
}

.today-info > div .title {
  float: left;
  font-weight: 700;
}

.today-info > div .value {
  float: right;
}

.week-list {
  list-style-type: none;
  padding: 0;
  margin: 10px 35px;
  -webkit-box-shadow: 0 0 50px -5px rgba(0, 0, 0, 0.25);
  box-shadow: 0 0 50px -5px rgba(0, 0, 0, 0.25);
  border-radius: 10px;
  /* background: #; */
}

.week-list > li {
  float: left;
  padding: 15px;
  cursor: pointer;
  -webkit-transition: 200ms ease;
  -o-transition: 200ms ease;
  transition: 200ms ease;
  border-radius: 10px;
}

.week-list > li:hover {
  -webkit-transform: scale(1.1);
  -ms-transform: scale(1.1);
  transform: scale(1.1);
  background: #fff;
  color: #222831;
  -webkit-box-shadow: 0 0 40px -5px rgba(0, 0, 0, 0.2);
  box-shadow: 0 0 40px -5px rgba(0, 0, 0, 0.2);
}

.week-list > li.active {
  background: #fff;
  color: #222831;
  border-radius: 10px;
}

.week-list > li .day-name {
  display: block;
  margin: 10px 0 0 0;
  text-align: center;
}

.week-list > li .day-icon {
  display: block;
  height: 30px;
  width: auto;
  margin: 0 auto;
}

.week-list > li .day-temp {
  display: block;
  text-align: center;
  margin: 10px 0 0 0;
  font-weight: 700;
}

.location-container {
  padding: 25px 35px;
}

.location-button {
  outline: none;
  width: 100%;
  -webkit-box-sizing: border-box;
  box-sizing: border-box;
  border: none;
  border-radius: 25px;
  padding: 10px;
  font-family: "Montserrat", sans-serif;
  background-image: var(--gradient);
  color: #ffffff;
  font-weight: 700;
  -webkit-box-shadow: 0 0 30px -5px rgba(0, 0, 0, 0.25);
  box-shadow: 0 0 30px -5px rgba(0, 0, 0, 0.25);
  cursor: pointer;
  -webkit-transition: -webkit-transform 200ms ease;
  transition: -webkit-transform 200ms ease;
  -o-transition: transform 200ms ease;
  transition: transform 200ms ease;
  transition: transform 200ms ease, -webkit-transform 200ms ease;
}

.location-button:hover {
  -webkit-transform: scale(0.95);
  -ms-transform: scale(0.95);
  transform: scale(0.95);
}

.location-button .feather {
  height: 1em;
  width: auto;
  margin-right: 5px;
}
</style>
