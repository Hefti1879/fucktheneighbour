<template>
  <img alt="Vue logo" src="./assets/jasskarten.jpg">
  <GameRound v-if="round" :players="players" :roundNo="round" :max-cards="maxCards"></GameRound>
  <AddPlayers v-else msg="Welcome to Your Vue.js App" @add-players="handleSetPlayers"/>
</template>

<script>
import AddPlayers from './components/AddPlayers.vue'
import GameRound from './components/GameRound.vue'

export default {
  name: 'App',
  components: {
    AddPlayers,
    GameRound
  },
  data() {
    return {
      players: [],
      round: 0,
      maxCards: 0
    }
  },
  methods: {
    handleSetPlayers(players) {
      // Do something with the players array
      this.players = players;
      console.log(players);
      this.round = 8;
      this.maxCards = Math.floor(36 / this.players.length);
    }
  }
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
