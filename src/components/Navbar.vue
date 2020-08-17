<template>
  <div id="navbar">
    <div class="logo" @click="toggleCountries">
      <span>Spoti</span>
      <img src="@/assets/SpotiFRESH.svg" alt="Fresh" class="FRESH" />
    </div>
    <div class="list" :class="show ? 'show' : 'not-show'">
      <country
        v-for="country in countries"
        :key="country"
        :id="country"
        @emit-country="sendCountry"
      />
    </div>
  </div>
</template>

<script>
import { markets } from "../consts"
import Country from "./Country"
export default {
  components: {
    Country,
  },
  data: function() {
    return {
      show: false,
      countries: markets,
    }
  },
  methods: {
    toggleCountries() {
      this.show = !this.show
    },
    sendCountry(countryCode) {
      this.toggleCountries()
      this.$emit("emit-country", countryCode)
    },
  },
}
</script>

<style scoped lang="scss">
@import url("https://fonts.googleapis.com/css2?family=Montserrat:wght@700&display=swap");
#navbar {
  color: black;
  font-family: "Montserrat", sans-serif;
  font-size: 4em;
  font-weight: 700;

  // background-color: #181818aa;
  padding-bottom: 0.1em;
  padding: 0 0.15em;
}
.logo {
  margin: 0 auto;
  display: flex;
  align-content: center;
  align-items: center;
  color: black;
  width: auto;
  align-items: center;
  justify-content: center;
  cursor: pointer;
}
.FRESH {
  z-index: 1;
  height: 1.5em;
  padding-top: 10px;
}

.show {
  display: block;
}
.not-show {
  display: none !important;
}
.list {
  background-color: #181818aa;
  color: #eee;
  // position: absolute;

  // width: 100%;
  // height: 100%;
  top: 120px;
  bottom: 0;
  margin: 5px;
  // margin: auto;
  z-index: 5;
  display: flex;
  flex-wrap: wrap;
}
</style>
