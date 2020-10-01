<template>
  <div id="app">
    <div id="nav">
      <router-link to="/">Home</router-link>
    </div>
    <router-view />
  </div>
</template>

<script>
import playersData from '@/data/players.json';
import tournamentsData from '@/data/tournaments.json';

const TOURNAMENT_TYPES = {
  1: 'allToAllTournament',
  2: 'groupTournament'
};
const POINTS_FOR_VICTORY = 3;

export default {
  name: 'App',
  components: {},
  data() {
    return {};
  },

  created() {
    this.init();
  },

  methods: {
    init() {
      this.prepareData();
    },

    prepareData() {
      tournamentsData.data.forEach((tournament) => {
        this[TOURNAMENT_TYPES[tournament.type]](tournament);
      });
    },

    allToAllTournament(tournament) {
      const playersSumm = tournament.participants.reduce((summ, playerId) => {
        const score = tournament.games.reduce((scoreRes, game) => {
          if (!game.players.includes(playerId)) return scoreRes;
          const playerIndex = game.players.indexOf(playerId);
          const opponentIndex = playerIndex === 0 ? 1 : 0;
          if (game.score[playerIndex] > game.score[opponentIndex]) scoreRes += POINTS_FOR_VICTORY;
          return scoreRes;
        }, 0);

        summ.push({ playerId, score });
        return summ;
      }, []);
      playersSumm
        .sort((a, b) => a.score - b.score)
        .forEach(({ playerId, score }) => {
          playersData[playerId].rating += +score / POINTS_FOR_VICTORY;
          playersData[playerId].history.push({ date: tournament.date, score: +score / 3 });
        });
      console.log(playersSumm);
    },

    groupTournament(tournament) {
      console.log('groupTournament -> tournament', tournament);
      console.log(tournamentsData);
      console.log(playersData);
    }
  }
};
</script>

<style lang="sass">
#app
  font-family: Avenir, Helvetica, Arial, sans-serif
  -webkit-font-smoothing: antialiased
  -moz-osx-font-smoothing: grayscale
  text-align: center
  color: #2c3e50

#nav
  padding: 30px

  a
    font-weight: bold
    color: #2c3e50

    &.router-link-exact-active
      color: #42b983
</style>
