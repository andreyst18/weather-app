<template>
  <div class="wrapper">
    <h2 class="card__city">{{ getCity }}</h2>
    <div class="card__row card__row--temperature">
      <img
        class="card__img"
        src="../assets/images/sunny.png"
        alt=""
        srcset=""
      />
      <div class="card__temperature">{{ getTemperature }}&#176;C</div>
    </div>
    <div class="card__row">
      Feels like {{ getFeelsLike }}&#176;C. {{ getWeatherDescription }}.
    </div>
    <div class="card__row">
      <div class="card__col card__col--wind">
        <!-- <img class="card__arrow" src="../assets/images/arrow.png" alt="arrow"> -->
        {{ getWindSpeed }} {{ getWindDegree }}
      </div>
      <div class="card__col card__col--pressure">
        <!-- <img class="card__pressure" src="../assets/images/pressure.png" alt="pressure"> -->
        {{ getPressure }}
      </div>
    </div>
    <div class="card__row">
      <div class="card__col"></div>
      <div class="card__col"></div>
    </div>
    <div class="card__row"></div>
  </div>
</template>

<script>
export default {
  name: "Card",
  data() {
    return {
      city: "",
      country: "",
      temperature: "",
      feelsLike: "",
      weatherDescription: "",
      windSpeed: 0,
      windDegree: 0,
      pressure: 0,
    };
  },

  beforeMount() {
    this.getData();
  },

  methods: {
    getData() {
      fetch(
        "https://api.openweathermap.org/data/2.5/weather?q=Moscow&units=metric&appid=62ce58e25a320fc5b56a8afac29380de"
      )
        .then((response) => response.json())
        .then((data) => {
          console.log(data);
          this.city = data.name;
          this.country = data.sys.country;
          this.temperature = data.main.temp;
          this.feelsLike = data.main.feels_like;
          this.weatherDescription = data.weather[0].description;
          this.windSpeed = data.wind.speed;
          this.windDegree = data.wind.deg;
          this.pressure = data.main.pressure;
        });
    },
  },

  computed: {
    getCity() {
      return `${this.city}, ${this.country}`;
    },

    getTemperature() {
      return Math.round(this.temperature);
    },

    getFeelsLike() {
      return Math.round(this.feelsLike);
    },

    getWeatherDescription() {
      return (
        this.weatherDescription.charAt(0).toUpperCase() +
        this.weatherDescription.slice(1)
      );
    },

    getWindSpeed() {
      return `${this.windSpeed.toFixed(1)}m/s`;
    },

    getWindDegree() {
      if (this.windDegree < 22.5) return "N";
      if (this.windDegree < 67.5) return "NE";
      if (this.windDegree < 112.5) return "E";
      if (this.windDegree < 157.5) return "SE";
      if (this.windDegree < 202.5) return "S";
      if (this.windDegree < 247.5) return "SW";
      if (this.windDegree < 292.5) return "W";
      if (this.windDegree < 337.5) return "NW";
      if (this.windDegree >= 337.5) return "N";
      return "";
    },

    getPressure() {
      return `${this.pressure}hPa`;
    },
  },
};
</script>

<style lang="scss" scoped>
@import "../styles/reset.css";

.wrapper {
  width: 200px;
  padding: 20px;
}

.card {
  &__city {
    font-weight: 700;
    margin-bottom: 30px;
  }

  &__row {
    display: flex;
    font-size: 14px;
    margin-bottom: 20px;
    line-height: 140%;
    align-items: center;

    &--temperature {
      justify-content: space-around;
      margin-bottom: 30px;
    }
  }

  &__img {
    width: 70px;
    height: 70px;
  }

  &__temperature {
    font-size: 50px;
    font-weight: 500;
  }

  &__col {
    display: flex;
    align-items: center;
    position: relative;

    &::before {
      position: absolute;
      left: 0;
      content: "";
    }

    &--wind {
      padding-left: 25px;
      margin-right: 20px;

      &::before {
        top: 0;
        background: url("../assets/images/arrow.png");
        background-size: cover;
        width: 15px;
        height: 15px;
        transform: rotate(225deg);
      }
    }

    &--pressure {
      padding-left: 25px;

      &::before {
        top: -5px;
        background: url("../assets/images/pressure.png");
        background-size: cover;
        width: 25px;
        height: 25px;
      }
    }
  }
}
</style>
