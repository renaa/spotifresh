<template>
  <div id="app">
    <spotify-card
      v-for="(album, index) in albums"
      :key="index"
      :song="album.name"
      :artist="getArtistDisplayName(album.artists)"
      :imgUrl="album.images[0].url"
      :outLink="getAlbumUri(album)"
    />
  </div>
</template>

<script>
import SpotifyCard from "./components/SpotifyCard.vue"
// import secret from "../.secret.js"
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
    getArtistDisplayName(artists) {
      let s = ""
      artists.forEach(element => {
        s += element.name + " ➕ "
      })
      return s.slice(0, -2)
    },
    getAlbumUri(album){
      return album.uri// + ":autoplay:1"
    }
  },

  created: function() {
    fetch("https://accounts.spotify.com/api/token", {
      body: "grant_type=client_credentials",
      headers: {
        Authorization:
          `Basic ZDVhNzNjYTUyOTk3NDVmZmI1OGVmN2RmMjllODUyN2Y6Y2FmZTUzNzZhZmRkNDA5YzgyY2E4YjQ4NTA1MjEwMzc=`,
        "Content-Type": "application/x-www-form-urlencoded",
      },
      method: "POST",
    })
    .then(response => response.json())
    .then(bearer => {
      fetch("https://api.spotify.com/v1/browse/new-releases?limit=50", {
      headers: {
        Accept: "application/json",
        Authorization:
          `Bearer ${bearer.access_token}`,
        "Content-Type": "application/json",
      },
    })
      .then(response => response.json())
      .then(result => {
        this.albums = result.albums.items
        this.gotResult = true
      })
      .catch(error => console.error(error));
  
    }).catch(error => console.error(error));
  }
}
</script>

<style lang="scss">
body {
  margin: 0;
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
