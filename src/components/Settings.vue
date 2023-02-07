<template>
  <div class="settings">
    <div class="settings__close" @click="$emit('settings-off')">
      <div class="settings__close--first"></div>
      <div class="settings__close--second"></div>
    </div>
    <h2 class="settings__title">Settings</h2>

    <ul class="settings__cities">
      <li
        class="settings__city"
        v-for="(city, index) in cities"
        :key="index"
        @click.stop="handleChoice"
      >
        <div>
          <img
            class="settings__icon settings__icon--drag"
            src="../assets/images/drag.png"
            alt="drag"
          />
          <span>{{ city }}</span>
        </div>
        <img
          class="settings__icon settings__icon--trash"
          src="../assets/images/trash.png"
          alt="trash"
        />
      </li>
    </ul>

    <div class="settings__row">
      <div class="settings__placeholder" v-if="cities.length === 0">
        Add up to 4 cities<br />
        to this list
      </div>
    </div>

    <div class="settings__add-city">
      <h2 class="settings__title">Add location:</h2>
      <div class="settings__row">
        <input
          class="settings__input"
          type="text"
          v-model="currentCity"
          @keyup.enter="inputHandler"
        />
        <img src="../assets/images/enter.png" alt="add" @click="inputHandler" />
      </div>
    </div>
    <div class="settings__alert" v-if="wrongValue">Сity not found</div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      cities: [],
      currentCity: "",
      wrongValue: false,
      selectedCity: "",
    };
  },

  methods: {
    addCity() {
      if (this.selectedCity && this.cities.length < 4) {
        this.cities.push(this.selectedCity);
        this.selectedCity = "";
        this.currentCity = "";
      }
    },

    async inputHandler() {
      try {
        const response = await fetch(
          `https://api.openweathermap.org/data/2.5/weather?q=${this.currentCity}&units=metric&appid=62ce58e25a320fc5b56a8afac29380de`
        );
        if (!response.ok) {
          throw new Error();
        }
        const data = await response.json();
        console.log(data);
        this.selectedCity = `${data.name}, ${data.sys.country}`;
        this.addCity();
      } catch (error) {
        this.showWarning();
      }
    },

    showWarning() {
      this.wrongValue = true;
      setTimeout(() => {
        this.wrongValue = false;
      }, 1000);
    },

    handleChoice() {
      this.setActive();
      console.log("qq");
      this.$emit("select-city", this.getCityValue);
    },

    setActive() {
      let items = document.querySelectorAll(".settings__city");
      for (let item of items) {
        if (item.classList.contains("settings__city--active")) {
          item.classList.remove("settings__city--active");
        }
      }
      event.target.closest("li").classList.add("settings__city--active");
    },
  },

  computed: {
    getCityValue() {
      let node = document.querySelector(".settings__city--active");
      return node.querySelector("span").innerHTML;
    },
  },
};
</script>

<style lang="scss" scoped>
@import "../styles/reset.css";

.settings {
  &__title {
    font-weight: 700;
  }

  &__add-city {
    font-size: 14px;
    position: absolute;
    bottom: 30px;
  }

  &__cities {
    margin-top: 20px;
    margin-bottom: 40px;
  }

  &__row {
    display: flex;
    justify-content: space-between;

    & img {
      width: 20px;
      height: 20px;
      padding-top: 10px;
      cursor: pointer;
    }
  }

  &__row {
    position: relative;
    display: flex;
    justify-content: center;
  }

  &__placeholder {
    position: absolute;
    top: 60px;
    font-weight: bold;
    font-size: 14px;
    opacity: 0.4;
    text-align: center;
    line-height: 140%;
  }

  &__input {
    margin-top: 10px;
    margin-right: 10px;
    width: 180px;
    height: 20px;
    background-color: #fafafa;
    border: 0;
    box-shadow: 0 0 4px rgba(0, 0, 0, 0.3);
    transition: 0.3s box-shadow;

    &:hover {
      box-shadow: 0 0 4px rgba(0, 0, 0, 0.5);
    }

    &:focus {
      outline: none;
    }
  }

  &__city {
    background: #d7dddf;
    padding: 7px 10px;
    display: flex;
    justify-content: space-between;
    margin-bottom: 15px;
    border-radius: 5px;
    cursor: default;

    &--active {
      background: #7b8ffd;
    }

    & div {
      display: flex;
      align-items: center;

      & img {
        margin-right: 10px;
      }
    }

    & span {
      margin-top: 2px;
    }
  }

  &__icon {
    width: 15px;
    height: 15px;

    &--drag {
      cursor: move;
    }

    &--trash {
      cursor: pointer;
    }
  }

  &__alert {
    position: absolute;
    bottom: 10px;
    color: #f54d4d;
    font-size: 12px;
    font-weight: bold;
  }

  &__close {
    position: absolute;
    top: 20px;
    right: 20px;
    cursor: pointer;

    &--first {
      position: relative;
      top: 2px;
      width: 15px;
      height: 2px;
      background: #000;
      transform: rotate(45deg);
    }

    &--second {
      width: 15px;
      height: 2px;
      background: #000;
      transform: rotate(-45deg);
    }
  }
}
</style>
