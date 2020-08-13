<template>
  <div class="card">
    <a :href="outLink">
      <img :src="imgUrl" alt="" />
    </a>
    <div class="info-line info-line-top">
      <p class="song">{{ song }}</p>
      <p class="info infotop">{{ album_type }}</p>
    </div>
    <div class="info-line">
      <p class="artist">{{ artist }}</p>
      <p class="info infobottom">{{ release_date }}</p>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    song: String,
    artist: String,
    imgUrl: String,
    outLink: String,
    release_date: String,
    album_type: String,
  },
}
</script>

<style scoped lang="scss">
.card {
  background-color: #181818aa;
  color: #eee;

  text-align: start;
  margin: 10px;
  padding: 5px;
  width: calc(33.33% - 30px);
  display: flex;
  flex-direction: column;
  align-items: center;
}
@media only screen and (max-width: 920px) {
  .card {
    width: calc(50% - 30px);
  }
}
img {
  width: 640px;
  max-width: 100%;
}
p {
  margin: 0.3em 0;
}
.song {
  font-weight: 600;
}
.info-line {
  display: flex;
  position: relative;
  width: 100%;
  align-items: center;
  justify-content: space-between;
  flex-wrap: wrap;
}

.info {
  color: #eee6;
}
</style>
