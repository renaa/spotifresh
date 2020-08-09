<template>
  <div id="app">
    <!-- <SpotifyCard
      :song="testSong"
      artist="JonDooe"
      imgUrl="https://i.pinimg.com/originals/4c/97/10/4c971038f52be5ecc2d3845f303edd64.jpg"

      :outLink="album.external_urls.spotify"

    /> -->

    <spotify-card v-for="(album, index) in albums" :key="index"
      :song="album.name"
      :artist="getArtistDisplayName(album.artists)"
      :imgUrl="album.images[0].url"
      :outLink="album.uri"
    />

   
  </div>
</template>

<script>
import SpotifyCard from "./components/SpotifyCard.vue"

export default {
  name: "App",
  components: {
    SpotifyCard,
  },
  data: function() {
    return {
      gotResult: false,
      albums: {},
      
    }
  },
  methods: {
    getArtistDisplayName(artists){
      let s = ''
      artists.forEach(element => {
        s+= element.name + " ➕ "
      });
      return s.slice(0, -2);
    }
  },
  //https://developer.spotify.com/console/get-new-releases/?country=&limit=50&offset=5
  created: function() {
    fetch("https://api.spotify.com/v1/browse/new-releases?limit=50&offset=5", {
      headers: {
        Accept: "application/json",
        Authorization:
          "Bearer BQBg3y0aqL6B-jHeVSi9MVKnWwLNXEz22T57BO21EjCagB6_6fcgnVlEg2kpXs0gYghtH1NZrO5P3HBUU_2idF2c0WJPGgDycP_g2oA0T-_INpynd2aZDwhcSNOhcjEeOVOAmDByD9ZXlnAc5PsYm_KKGMDkdg",
        "Content-Type": "application/json",
      },
    })
      .then(response => response.json())
      .then(result => {
        this.albums = result.albums.items
        // this.album = this.albums[0]
        this.gotResult = true
        
      })
      .catch(error => console.error(error))
  },
}
</script>

<style lang="scss">
body{
  margin:0;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  background-color: #2c3e50;

  display: flex;
  flex-wrap: wrap;
}
</style>
