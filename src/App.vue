<template>
  <div id="app">
    <navbar />
    <div class="cards-container">
      <spotify-card
        v-for="(album, index) in albums"
        :key="index"
        :song="album.name"
        :artist="getArtistDisplayName(album.artists)"
        :imgUrl="album.images[0].url"
        :outLink="getAlbumUri(album)"
        :release_date="getAlbumReleaseDate(album.release_date)"
        :album_type="album.album_type"
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
        s += element.name + " | "
      })
      return s.slice(0, -2)
    },
    getAlbumUri(album) {
      return album.uri
    },
    getAlbumReleaseDate(albumdate) {
      const monthNames = [
        "January",
        "February",
        "March",
        "April",
        "May",
        "June",
        "July",
        "August",
        "September",
        "October",
        "November",
        "December",
      ]
      let date = new Date(Date.parse(albumdate))
      return date.getDate() + ". " + monthNames[date.getMonth()]
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
  background-color: #80cc74;
  margin: 0;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  background-color: #80cc74;

  background: linear-gradient(180deg, #80cc74, #7ecaba, #af67d7);
  background-size: 600% 600%;

  -webkit-animation: AnimationName 15s ease infinite;
  -moz-animation: AnimationName 15s ease infinite;
  -o-animation: AnimationName 15s ease infinite;
  animation: AnimationName 15s ease infinite;
}

@-webkit-keyframes AnimationName {
  0% {
    background-position: 49% 0%;
  }
  50% {
    background-position: 52% 100%;
  }
  100% {
    background-position: 49% 0%;
  }
}
@-moz-keyframes AnimationName {
  0% {
    background-position: 49% 0%;
  }
  50% {
    background-position: 52% 100%;
  }
  100% {
    background-position: 49% 0%;
  }
}
@-o-keyframes AnimationName {
  0% {
    background-position: 49% 0%;
  }
  50% {
    background-position: 52% 100%;
  }
  100% {
    background-position: 49% 0%;
  }
}
@keyframes AnimationName {
  0% {
    background-position: 49% 0%;
  }
  50% {
    background-position: 52% 100%;
  }
  100% {
    background-position: 49% 0%;
  }
}
.cards-container {
  text-align: center;
  display: flex;
  flex-wrap: wrap;
}
</style>
