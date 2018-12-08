<template>
  <div class="scene">
    <dealer ref="dealer" @now="now" @result="end" :tramp="tramp"/>
    <div class="central">
      <div class="status">
        <p>Player：{{plResult}}</p>
      </div>
      <div class="status">
        <p>Dealer：{{dlResult}}</p>
      </div>
    </div>
    <player @stand="stand" @now="now" :isButton="isButton" :tramp="tramp"/>
    <button class="button is-info result" @click="retry()" v-if="result !== ''">
      {{ result }}<br>
      もう一度プレイ
    </button>
  </div>
</template>

<script>
import Dealer from './Dealer'
import Player from './Player'

export default {
  name: "blackjack",
  components: { 
    Dealer,
    Player 
  },
  data () {
    return {
      gameMessage: 'Black Jack App',
      plResult: '-',
      dlResult: '-',
      isButton: true,
      tramp: []
    }
  },
  methods: {
    stand (plResult) {
      this.plResult = plResult
      this.$refs.dealer.$emit('draw', String(plResult).indexOf('Bust') > -1 )
    },
    now (points, target) {
      target === 'player' ? this.plResult = points : this.dlResult = points
    },
    end (dlResult) {
      this.dlResult = dlResult
      this.isButton = false
    },
    retry () {
      location.reload()
    }
  },
  created () {
    // トランプ：ジョーカー除くカードの生成
    for (var i=0; i<4; i++){
      for (var j=1; j<14; j++){
        this.tramp.push(
          {
            'mark': i,
            'value': j
          }
        )
      }
    }
  },
  computed: {
    result: function () {
      if (this.isButton) return ''
      if (this.plResult > this.dlResult || String(this.dlResult).indexOf('Bust') > -1) return 'あなたの勝ちです'
      if (this.plResult < this.dlResult || String(this.plResult).indexOf('Bust') > -1) return 'あなたの負けです'
      return '引き分けです'
    }
  }
}
</script>
<style scoped>
.scene {
  overflow: hidden;
  height: 100vh;
  width: 100vw;
  background-color: green;
}
.central {
  margin: 10%;
  display: -webkit-flex;
  display: -moz-flex;
  display: -ms-flex;
  display: -o-flex;
  display: flex;
  justify-content: center;
}
.status {
  width: 200px;
  height: 50px;
  padding: 15px;
  background-color: white;
}
.result {
  position: fixed;
  bottom: 10vh;
  left: 10vw;
  width: 80vw;
  height: 10vh;
  font-size: 3vh;
  font-weight: 900;
}
</style>