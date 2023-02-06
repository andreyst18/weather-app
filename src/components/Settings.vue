<template>
  <div>
    <h2 class="settings__title">Settings</h2>
    <div class="settings__add-city">
      <h2 class="settings__title">Add location:</h2>
      <div class="settings__row">
        <input
          class="settings__input"
          type="text"
          v-model="currentCity"
          @keyup.enter="addCity"
        />
        <img src="../assets/images/enter.png" alt="add" @click="addCity" />
      </div>
    </div>
    <ul class="settings__cities">
      <li class="settings__city" v-for="(city, index) in cities" :key="index">
        <img src="../assets/images/drag.png" alt="drag" />
        {{ city }}
        <img src="../assets/images/trash.png" alt="trash" />
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  data() {
    return {
      cities: [],
      currentCity: "",
    };
  },

  methods: {
    addCity() {
      if (this.currentCity) {
        this.cities.push(this.currentCity);
        this.currentCity = "";
      }
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
    margin-top: 30px;
    font-size: 14px;
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

  &__input {
    margin-top: 10px;
    width: 185px;
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
    padding: 5px 10px;
    display: flex;
    justify-content: space-between;
    margin-bottom: 15px;
    border-radius: 5px;

    & img {
      width: 15px;
      height: 15px;
      cursor: pointer;
    }
  }
}
</style>
