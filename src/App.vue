<template lang="pug">
#app
      img(src='https://jihenaor.github.io/musicvue/assets/logo.png')
      h1 {{ msg }}
      h2 Platzi logo

      select(v-model="selectCountry")
        option(v-for="country in countries" v-bind:value="country.value") {{ country.name}}

      spinner(v-show="loading")

      ul
        artist(v-for="artist in artists" v-bind:artist="artist" v-bind:key="artist.mbid")
</template>

<script>
import Artist from './components/Artist.vue'
import getArtists from './api'
import Spinner from './components/Spinner.vue'

export default {
  name: 'app',
  data () {
    return {
      msg: 'Welcome to Your Vue.js App !',
      artists: [ ],
      countries: [
        { name: 'Argentina', value: 'argentina'},
        { name: 'Colombia', value: 'colombia'},
        { name: 'España', value: 'spain'},
      ],
      selectCountry: 'argentina',
      loading: true
    }
  },
  components: {
    Artist,
    Spinner
  },
  methods: {
    refreshArtists() {
      const self = this
      self.loading = true;
      this.artists = [];
      getArtists(this.selectCountry)
        .then( function (artists) {
          self.loading = false;
          self.artists = artists;
        })
    }
  },
  mounted() {
    this.refreshArtists();
  },
  watch: {
    selectCountry() {
      this.refreshArtists();
    }
  }
}
</script>

<style lang="stylus">
#app 
	font-family 'Avenir', Helvetica, Arial, sans-serif
	-webkit-font-smoothing antialiased
	-moz-osx-font-smoothing grayscale
	text-align center
	color #2c3e50
	margin-top 60px
h1, h2 
	font-weight normal
ul 
	list-style-type none
	padding 0
li 
	display inline-block
	margin 0 10px
a 
	color #42b983

</style>
