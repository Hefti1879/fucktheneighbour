<template>
    <div class="container">
        <h3 class="center-align">Player Scores</h3>
        <table>
            <thead>
                <tr>
                    <th>Player Name</th>
                    <th class="l">Score</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="(player, index) in sortedPlayers()" :key="index">
                    <td>{{ player.name }}</td>
                    <td class="l">{{ player.points }}</td>
                </tr>
            </tbody>
        </table>
    </div>
</template>
  
<script>
export default {
    props: {
        players: { type: Array, required: true }
    },
    methods: {
        sortedPlayers() {
            let sortedPlayers = this.players;
            return sortedPlayers.sort((a, b) => b.score - a.score);
        }
    }
};
</script>
  
<style scoped>
.container {
    margin-top: 50px;
}

.center-align {
    text-align: center;
}

table {
    border-spacing: 1;
    border-collapse: collapse;
    background: white;
    border-radius: 6px;
    overflow: hidden;
    max-width: 800px;
    width: 100%;
    margin: 0 auto;
    position: relative;
}

table * {
    position: relative;
}

table td,
table th {
    padding: 5px 15px;
}

table thead tr {
    background: #70bb5d;
    font-size: 16px;
    height: 30px;
}

table tbody tr {
    border-bottom: 1px solid #E3F1D5;
}

table td,
table th {
    text-align: left;
}

table td.l,
table th.l {
    text-align: right;
}

@media screen and (max-width: 600px) {

    table tbody tr {
        height: auto;
        padding: 8px 0;
    }


    table tbody tr td:last-child {
        margin-bottom: 0;
    }


}</style>