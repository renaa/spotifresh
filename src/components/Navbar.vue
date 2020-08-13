<template>
  <div id="navbar">
    <div class="logo">
      <span>Spoti</span>
      <img src="@/assets/SpotiFRESH.svg" alt="Fresh" class="FRESH" />
    </div>
  </div>
</template>

<script>
export default {}
</script>

<style scoped lang="scss">
@import url("https://fonts.googleapis.com/css2?family=Montserrat:wght@700&display=swap");
#navbar {
  color: black;
  font-family: "Montserrat", sans-serif;
  font-size: 4em;
  font-weight: 700;
  display: flex;
  // background-color: #181818aa;
  padding-bottom: 0.1em;
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
}
.FRESH {
  // position: absolute;
  z-index: 4;

  height: 1.5em;
  padding-top: 10px;
}
</style>
