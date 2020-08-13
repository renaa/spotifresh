<template>
  <div id="app">
    <navbar/>
    <div class="cards-container">
    <spotify-card
      v-for="(album, index) in albums"
      :key="index"
      :song="album.name"
      :artist="getArtistDisplayName(album.artists)"
      :imgUrl="album.images[0].url"
      :outLink="getAlbumUri(album)"
      :release_date="album.release_date"
      :type="album.album_type"
    />
    </div>
  </div>
</template>

<script>
import Navbar from "./components/Navbar.vue"
import SpotifyCard from "./components/SpotifyCard.vue"
export default {
  name: "App",
  components: {
    Navbar,
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
    getAlbumUri(album) {
      return album.uri
    },
  },
  beforeCreate: async function() {
    try {
      let tokenUrl = "https://accounts.spotify.com/api/token"
      let requestObject = {
        body: "grant_type=client_credentials",
        headers: {
          Authorization: `Basic ${process.env.VUE_APP_B64}`,
          "Content-Type": "application/x-www-form-urlencoded",
        },
        method: "POST",
      }

      let tokenResponse = await fetch(tokenUrl, requestObject)
      let bearer = await tokenResponse.json()
      console.log(requestObject)
      let appUrl = "https://api.spotify.com/v1/browse/new-releases?limit=50"
      let appRequestObject = {
        headers: {
          Accept: "application/json",
          Authorization: `Bearer ${bearer.access_token}`,
          "Content-Type": "application/json",
        },
      }

      let appResponse = await fetch(appUrl, appRequestObject)
      let result = await appResponse.json()

      console.log(result)
      this.albums = result.albums.items
      this.gotResult = true
    } catch (error) {
      console.error(error)
    }
  },
}
</script>

<style lang="scss">
body {
  background-color: #2c3e50;
  margin: 0;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
.cards-container{

  text-align: center;
  display: flex;
  flex-wrap: wrap;

}
</style>
