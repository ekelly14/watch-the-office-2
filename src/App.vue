<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <!-- <HelloWorld msg="Welcome to Your Vue.js App"/> -->
    <button @click="addShowToList">
      Activate
    </button>
    {{ showList }}
    <br/>
    <Artplayer @get-instance="getInstance" :option="option" :style="style" />
  </div>
</template>

<script>
// import HelloWorld from './components/HelloWorld.vue'
import Artplayer from "artplayer/examples/vue/Artplayer"

export default {
  name: 'App',
  components: {
    // HelloWorld
    Artplayer
  },
  data(){
    return{
      showList: {},
      option: {
        url: "https://artplayer.org/assets/sample/video.mp4", 
        fullscreen: true,
        pip: true,

      },
      style: {
        width: "600px",
        height: "400px",
        margin: "60px auto 0"
      }
    }
  },
  created(){
    this.showList = this.getShowsFromFile()
    
  },
  methods: {
    getShowsFromFile() {
      if(window && window.require){
        const fs = window.require('fs')
        const rawShows = fs.readFileSync('./shows.json')
        console.log('obj', JSON.parse(rawShows))
        return JSON.parse(rawShows)
      }
    },
    addShowToList(){
      if(window && window.require){
        const fs = window.require('fs')
        let currentShows = this.getShowsFromFile()
        const content = {
          name: 'The_Office',
          displayName: 'The Office',
          path: 'file:://'
        }
        currentShows.push(content)
        console.log('currentShows', currentShows)
        fs.writeFileSync('./shows.json', JSON.stringify(currentShows))
        this.showList = currentShows
        console.log(this.getShowsFromFile())
      }
    },
    getInstance(art){
      console.log(art)
    }
  },
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
