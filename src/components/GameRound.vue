<template>
    <h2>{{ roundNo }}. Runde, {{ cards }} Karte(n)</h2>
    <form v-if="!betsPlaced" class="game-round-form" @submit.prevent="finishBets">
      <h3>Stiche ansagen</h3>
      <div class="player-row" v-for="(player, index) in players" :key="index">
        <label :for="'player-bets-' + index">{{ player.name }}:</label>
        <input :min="0" :max="cards" :id="'player-bets-' + index" type="number" v-model="player.bets">
        <button class="increase-btn" @click.prevent="if(player.bets < cards) player.bets++;">
          <i class="material-icons">add</i>
        </button>
      </div>
      <button class="btn submit-btn" type="submit">Runde Starten</button>
    </form>
    <form v-else class="game-round-form" @submit.prevent="finishRound">
      <h3>Gemachte Stiche</h3>
      <div class="player-row" v-for="(player, index) in players" :key="index">
        <label :for="'player-wins-' + index">{{ player.name }}:</label>
        <input :min="0" :max="cards" :id="'player-wins-' + index" type="number" v-model="player.wins">
        <button class="increase-btn" @click.prevent="if(player.wins < cards) player.wins++;">
          <i class="material-icons">add</i>
        </button>
      </div>
      <button class="btn submit-btn" type="submit">Nächste Runde</button>
    </form>       
</template>

<script>
export default {
  name: 'GameRound',
  props: {
    players: Array,
    roundNo: Number,
    maxCards: Number
  },
  emits : ['finishRound'],
  data() {
    return {
        cards: 1,
        betsPlaced: false
    }
  },
  created : function(){
    this.resetBets();
    this.calculateCards(this.roundNo);
  },
  methods: {
    calculateCards(round){
      this.cards = this.maxCards - Math.abs(((round - 1 - (2*(this.maxCards-1)*Math.floor((round-2)/2/(this.maxCards-1)))) % (2*(this.maxCards)-1)) - this.maxCards+1)
    },
    resetBets(){
      this.players.forEach(p => {
        p.bets = 0;
        p.wins = 0;
    })
    },
    finishBets(){
      console.log(this.players)
      let bets = 0;
      this.players.forEach(p => {
        bets += p.bets;
      });
      if(bets == this.cards){
        alert("Ungültige Anzahl angesagter Stiche");
        return;
      }
      this.betsPlaced = true;
    },
    finishRound(){
      let wins = 0;
      this.players.forEach(p => {wins += p.wins});
      if(wins != this.cards){
        alert("Es sind zu viele/zu wenige Stiche eingetragen");
        return;
      }
      let round = {
        'players': this.players,
        'roundNo': this.roundNo
      }
      this.$emit('finish-round', round);
      this.startRound();
    },
    startRound(){
      this.resetBets();
      this.calculateCards(this.roundNo+1);
      this.betsPlaced = false;
    }
  }
}
</script>

<style scoped>
.game-round-form {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin: 5px;
}

.player-row {
  display: flex;
  align-items: center;
  margin: 10px 0;
}

label {
  width: 80px;
}

input {
  width: 60px;
  margin: 0 10px;
}

.increase-btn {
  border: none;
  background: transparent;
  cursor: pointer;
  padding: 0;
}
@media (max-width: 768px) {
  .game-round-form {
    align-items: stretch;
  }

  label {
    font-size: 20px;
    text-align: left;
  }
  
  input {
    margin-left: auto;
    font-size: 20px;
  }
  
}
</style>