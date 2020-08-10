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
  // name: "HelloWorld",
  props: {
    song: String,
    artist: String,
    imgUrl: String,
    outLink: String
  },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
.card {
  background-color: #181818aa;
  color: #eee;

  text-align: start;
  margin: 10px;
  padding: 5px;
  max-width: 30%;

  img {
    width: 640px;
    max-width: 100%;
  }
  p {
    margin: 0.3em 0;
  }
  .song{
    font-weight: 600;
    margin-top: 0.6em;
  }
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
