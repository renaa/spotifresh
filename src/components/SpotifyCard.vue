<template>
  <div class="card">
    <a :href="outLink">
      <img :src="imgUrl" alt="" />
    </a>
    <p class="song">{{ song }}</p>
    <p class="artist">{{ artist }}</p>
  </div>
</template>

<script>
export default {
  props: {
    song: String,
    artist: String,
    imgUrl: String,
    outLink: String,
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
  margin-top: 0.6em;
}
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: unset;
  text-decoration: none;
  display: block;
  z-index: 3;
}
</style>
