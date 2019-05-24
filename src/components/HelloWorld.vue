<template>
  <div class="hello">
    <table v-for="game in scoreboards" v-bind:key="game.date">
      <thead>
      <tr>
        <td colspan="7">{{ game.date}}</td>
      </tr>
      <tr>
        <td></td>
        <td>1</td>
        <td>2</td>
        <td>3</td>
        <td>4</td>
        <td>5</td>
        <td>T</td>
      </tr>
      </thead>
      <tbody>
      <tr>
        <td>{{game.home.abbr}}</td>
        <td>{{game.home.score['1']}}</td>
        <td>{{game.home.score['2']}}</td>
        <td>{{game.home.score['3']}}</td>
        <td>{{game.home.score['4']}}</td>
        <td>{{game.home.score['5']}}</td>
        <td>{{game.home.score['T']}}</td>
      </tr>
      <tr>
        <td>{{game.away.abbr}}</td>
        <td>{{game.away.score['1']}}</td>
        <td>{{game.away.score['2']}}</td>
        <td>{{game.away.score['3']}}</td>
        <td>{{game.away.score['4']}}</td>
        <td>{{game.away.score['5']}}</td>
        <td>{{game.away.score['T']}}</td>
      </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  data () {
    return {
      teams: null,
      scoreboards: []
    }
  },
  methods: {
    updateScoreboard: function () {
      let api = 'http://www.nfl.com/liveupdate/scores/scores.json' // 'http://www.nfl.com/liveupdate/scores/scores.json' // '/data/demo.json'
      this.axios
        .get(api, { responseType: 'json' })
        .then(response => {
          let raw = response.data
          console.log(raw)
          for (let d in raw) {
            let game = {
              date: d,
              home: raw[d].home,
              away: raw[d].away
            }
            this.scoreboards.push(game)
          }
        })
        .finally(() => console.log(this.scoreboards[0]))
    }
  },
  mounted () {
    this.axios.get('/data/teams.json', { responseType: 'json' })
      .then(response => {
        this.teams = response.data
      }).finally(() => this.updateScoreboard())
  }
}

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
