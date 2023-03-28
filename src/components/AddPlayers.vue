<template>
  <div class="add-players">
    <h2 class="add-players-title">Add Players</h2>
    <form class="add-players-form" @submit.prevent="addPlayers">
      <div class="input-field" v-for="(player, index) in players" :key="index">
        <input :id="'player-name-' + index" type="text" v-model="player.name" required>
        <label :for="'player-name-' + index" class="add-players-label" v-if="!player.name">Player {{ index + 1 }}:</label>
        <button class="remove-player-btn" @click.prevent="removePlayer(index)">
          <i class="material-icons">close</i>
        </button>
        <span class="focus-border"></span>
      </div>
      <button class="btn add-player-btn" type="button" v-if="players.length < 10" @click="addPlayer">Add Player</button>
      <button class="btn add-player-btn" type="submit">Finish</button>
    </form>
  </div>
</template>

<script>
export default {
  name: 'AddPlayers',
  props: {
    msg: String
  },
  data() {
    return {
      players: [
        { id: 1, name: '', points: 0},
        { id: 2, name: '', points: 0},
        { id: 3, name: '', points: 0}
      ]
    }
  },
  methods: {
    addPlayer() {
      const newPlayerId = this.players.length + 1
      this.players.push({ id: newPlayerId, name: '' })
    },
    addPlayers() {
      this.$emit('add-players', this.players)
    },
    removePlayer(index) {
      this.players.splice(index, 1)
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.add-players {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 20px;
}

.add-players-title {
  font-size: 28px;
  font-weight: bold;
  margin-bottom: 20px;
}

.add-players-form {
  display: flex;
  flex-direction: column;
  /*align-items: center;*/
  width: 70%;
}

.input-field {
  position: relative;
  margin-bottom: 20px;
}

.input-field input {
  font-size: 16px;
  padding: 10px 0;
  width: 100%;
  border: none;
  border-bottom: 2px solid #ccc;
}

.input-field input:focus {
  outline: none;
}

.add-players-label {
  position: absolute;
  top: 10px;
  left: 0;
  font-size: 16px;
  color: #666;
  transition: all 0.2s ease-in-out;
}

.input-field input:focus~.add-players-label {
  display: none;
}

.remove-player-btn {
  border: none;
  background: transparent;
  position: absolute;
  right: 0;
  top: 15%;
  cursor: pointer;
  padding: 0;
}

.focus-border {
  position: absolute;
  bottom: 0;
  left: 0;
  width: 100%;
  height: 2px;
  background-color: #2196f3;
  transform: scaleX(0);
  transition: all 0.2s ease-in-out;
}

.input-field input:focus~.focus-border
{
  transform: scaleX(1);
}

.add-player-btn {
  margin-top: 20px;
  background-color: #2196f3;
  color: #fff;
  border: none;
  border-radius: 5px;
  padding: 10px 20px;
  font-size: 16px;
  cursor: pointer;
  transition: all 0.2s ease-in-out;
}

.add-player-btn:hover {
  background-color: #1976d2;
}

@media (max-width: 600px) {
  .add-players {
    padding: 10px;
  }

  .add-players-title {
    font-size: 24px;
    margin-bottom: 10px;
  }

  .input-field {
    margin-bottom: 10px;
  }

  .input-field input {
    font-size: 14px;
    padding: 8px 0;
    border-bottom: 1px solid #ccc;
  }

  .add-players-label {
    top: 8px;
    font-size: 14px;
  }

  .focus-border {
    height: 1px;
  }

  .add-player-btn {
    margin-top: 10px;
    font-size: 14px;
    padding: 8px 16px;
  }
}
</style>
