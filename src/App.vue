<template>
  <div id="app">
    <img src="./assets/logo.png">
    <h2>Platzi Music</h2>
    <select v-model="selectedCountry">
      <option v-for="country in countries" :value="country.value">{{ country.name }}</option>
    </select>
    <spinner v-show="loading"></spinner>
    <ul>
      <artist v-for="artist in artists" v-bind:artist="artist" v-bind:key="artist.mbid"></artist>
    </ul>

  </div>
</template>

<script>
import Artist from './components/Artist'
import Spinner from './components/Spinner'
import getArtists from './api'

export default {
  name: 'app',
  data () {
    return {
      artists: [],
      countries: [
        {
          'value': 'chile',
          'name': 'Chile',
        },
        {
          'value': 'colombia',
          'name': 'Colombia',
        },
        {
          'value': 'argentina',
          'name': 'Argentina',
        },
        {
          'value': 'spain',
          'name': 'España',
        }
      ],
      selectedCountry: 'chile',
      loading: true,
    }
  },
  components: {
    Artist,
    Spinner
  },
  methods: {
    refreshArtists(){
      const self = this
      this.loading = true
      this.artists = []
      getArtists(this.selectedCountry)
        .then(function (artists){
          self.artists = artists
          self.loading = false
        })
    }
  },
  mounted: function(){
    this.refreshArtists()
  },
  watch: {
    selectedCountry(){
      this.refreshArtists()
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

h1, h2 {
  font-weight: normal;
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
  color: #42b983;
}
</style>
