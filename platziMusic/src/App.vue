<template lang="pug">
  #app
    img(alt='Vue logo', src='./assets/logo.png')
    h1 {{ msg }}
    select(v-model="selectedCountry")
      option(v-for="country in countries" v-bind:value="country.value") {{ country.name }}
    Loader(v-if="loading")
    ul(v-else) 
      Artist(v-for="artist in artists" v-bind:artist="artist" v-bind:key="artist.mbid")
</template>

<script>
import getArtists from './api';
import Artist from './components/Artist.vue';
import Loader from './components/Loader.vue'

export default {
  name: 'app',
  components: {
    Artist,
    Loader
  },
  data(){
    return{
      msg: 'PlatziMusic',
      artists: [],
      countries: [
        {name: 'Argentina', value: 'argentina'},
        {name: 'Colombia', value: 'colombia'},
        {name: 'España', value: 'spain'},
        {name: 'Peru', value: 'peru'}
      ],
      selectedCountry: 'Argentina',
      loading: true
    }
  },
  methods:{
    refreshArtists(){
      const self = this;
      this.loading = true;
      getArtists(this.selectedCountry)
      .then(function(artists){
        self.artists = artists;
        self.loading = false;
      })
    }
  },
  mounted: function(){
    this.refreshArtists();    
  },
  watch: {
    selectedCountry: function(){
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
    margin-top 20px

h3
    margin 40px 0 0

ul
    list-style-type none
    padding 0

li
    display inline-block
    margin 0 10px

a
    color #42b983
</style>
