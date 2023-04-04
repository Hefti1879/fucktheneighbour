<template>
  <img alt="Vue logo" src="./assets/jasskarten.jpg">
  <div v-if="round">
    <GameRound :players="players" :roundNo="round" :max-cards="maxCards" @finish-round="handleFinishRound"></GameRound>
    <PlayerScores :players="players" />
    <button class="btn submit-btn" @click.prevent="restartGame()" type="button">Neustart</button>
  </div>
  <AddPlayers v-else @add-players="handleSetPlayers"/>
</template>

<script>
import AddPlayers from './components/AddPlayers.vue'
import GameRound from './components/GameRound.vue'
import PlayerScores from "./components/PlayerScores.vue";

export default {
  name: 'App',
  components: {
    AddPlayers,
    GameRound,
    PlayerScores
  },
  data() {
    return {
      players: [],
      round: 0,
      maxCards: 0,
      rounds: []
    }
  },
  created : function(){
    let playersJSON = localStorage.getItem('players');
    let storedPlayers = JSON.parse(playersJSON);
    if(storedPlayers != null && storedPlayers.length){
      this.players = storedPlayers;
      this.rounds = JSON.parse(localStorage.getItem('rounds'));
      this.round = this.rounds.length+1;
      this.maxCards = Math.floor(36 / this.players.length);
    }
  },
  methods: {
    restartGame(){
      if(confirm("Spielstand zurücksetzen?")){
        this.round = 0;
        this.rounds = [];
      }
    },
    handleSetPlayers(players) {
      // Do something with the players array
      this.players = players;
      console.log(players);
      this.round = 1;
      this.maxCards = Math.floor(36 / this.players.length);
      this.saveData();
    },
    handleFinishRound(round){
      this.rounds.push(round);
      this.round = this.rounds.length + 1;
      round.players.forEach(p => {
        this.players.find(pl => pl.id == p.id).points += this.calculatePoints(p);
      });
      console.log(this.players);
      this.saveData();
    },
    saveData(){
      let playersJSON = JSON.stringify(this.players);
      localStorage.setItem('players', playersJSON);
      
      let roundJSON = JSON.stringify(this.rounds);
      localStorage.setItem('rounds', roundJSON);
    },
    calculatePoints(playerRound){
      let bets = playerRound.bets;
      let wins = playerRound.wins;
      return (bets == wins ? (bets == 0 ? 1 : bets * 10) : -(Math.abs(bets-wins) * 10))
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
.submit-btn {
  margin-top: 10px;
  padding: 10px 20px;
  font-size: 16px;
  background-color: #4caf50;
  color: #fff;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}
.submit-btn:hover {
  background-color: #5fbd64;
}
@media (max-width: 768px) {  
  button {
    font-size: 16px;
  }
  .submit-btn {
    margin-top: 10px;
    padding: 8px 16px;
  }
}
</style>
