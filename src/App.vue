<template lang="pug">
  #app
    img(src='./assets/logo.png')
    select(v-model="selectedCountry")
      option(v-for="country in countries" v-bind:value="country.value") {{ country.name }}
    spinner(v-show="loader")
    artist(v-for="artist in artists" :artist="artist" :key="artists.mbid")
</template>

<script>

import Artist from  './components/Artist.vue'
import Spinner from './components/Spinner.vue'
import getArtists from './api'

export default {
  name: 'app',  
  data () {
    return {
      artists : [],
      countries :[
        {name: "Argentina", value: "argentina"},
        {name: "Colombia", value: "colombia"},
        {name: "España", value: "spain"},
      ],
      selectedCountry: 'argentina',
      loader: true
    }
  },
  components: {
    Artist,
    Spinner
  },
  methods:{
    refreshArtist (){
      const self = this
      self.artists=[]
      self.loader=true
      getArtists(this.selectedCountry)
        .then(function (artists){
          self.loader=false
          self.artists = artists
      })
    }
  },
  mounted (){
    this.refreshArtist()
  },
   watch: {
    selectedCountry() {
      this.refreshArtist()
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
