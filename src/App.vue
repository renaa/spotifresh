<template>
  <div id="app">
    <navbar @emit-country="changeAlbumsBasedOnCountry" />
    <div class="cards-container">
      <spotify-card
        
        v-for="(album, index) in albums"
        :key="index"
        :song="album.name"
        :artist="getArtistDisplayName(album.artists)"
        :imgUrl="album.images[0].url"
        :outLink="getAlbumUri(album)"
      />
    </div>

    <!-- 
        :album_type="album.album_type"
        :release_date="getAlbumReleaseDate(album.release_date)"
         -->
  </div>
</template>

<script>
import Navbar from "./components/Navbar.vue"
import SpotifyCard from "./components/SpotifyCard.vue"
import { monthNames } from "./consts"
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
      accessToken: "",
      requestObject: {
        body: "grant_type=client_credentials",
        headers: {
          Authorization: `Basic ${process.env.VUE_APP_B64}`,
          "Content-Type": "application/x-www-form-urlencoded",
        },
        method: "POST",
      },
    }
  },
  methods: {
    getArtistDisplayName(artists) {
      let s = ""
      artists.forEach(element => {
        s += element.name + " | "
      })
      return s.slice(0, -3)
    },
    getAlbumUri(album) {
      return album.uri
    },
    getAlbumReleaseDate(albumdate) {
      let date = new Date(Date.parse(albumdate))
      return date.getDate() + ". " + monthNames[date.getMonth()]
    },
    async changeAlbumsBasedOnCountry(countryCode) {
      await this.albumsFetch(
        `https://api.spotify.com/v1/browse/new-releases?country=${countryCode}`
      )
    },
    async albumsFetch(url) {
      try {
        let appUrl = url
        let appRequestObject = {
          headers: {
            Accept: "application/json",
            Authorization: `Bearer ${this.accessToken}`,
            "Content-Type": "application/json",
          },
        }

        let appResponse = await fetch(appUrl, appRequestObject)
        let result = await appResponse.json()

        // console.log(result)
        this.albums = result.albums.items
        this.gotResult = true
      } catch (error) {
        console.log(error)
      }
    },
  },
  beforeCreate: async function() {
    let tokenUrl, requestObject
    try {
      tokenUrl = "https://accounts.spotify.com/api/token"
      requestObject = {
        body: "grant_type=client_credentials",
        headers: {
          Authorization: `Basic ${process.env.VUE_APP_B64}`,
          "Content-Type": "application/x-www-form-urlencoded",
        },
        method: "POST",
      }
    } catch (error) {
      console.log(error)
    }

    let tokenResponse = await fetch(tokenUrl, requestObject)
    let bearer = await tokenResponse.json()
    this.accessToken = bearer.access_token
    await this.albumsFetch("https://api.spotify.com/v1/browse/new-releases?limit=50")
  },
}
</script>

<style lang="scss">
@import url("https://fonts.googleapis.com/css2?family=Montserrat:wght@700&display=swap");

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
  margin: 10px;
  margin-bottom: 0;
  justify-content: space-evenly;
}
</style>
