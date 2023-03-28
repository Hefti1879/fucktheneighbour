<template>
    <h2>{{ roundNo }}. Round, Players: {{ this.players.length }}, {{ cards }} Card(s)</h2>
</template>

<script>
export default {
  name: 'GameRound',
  props: {
    players: Array,
    roundNo: Number,
    maxCards: Number
  },
  data() {
    return {
        cards: this.maxCards - Math.abs(((this.roundNo - 1) % (2*(this.maxCards)-1)) - this.maxCards+1)
    }
  },
  methods: {
    finishBets(){

    },
    updatePoints(){

    }
  }
}
</script>