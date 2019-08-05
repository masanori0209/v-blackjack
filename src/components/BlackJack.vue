<template>
  <!-- ゲームの勝敗 / プレーヤーの配置 / リザルト画面 -->
  <div class="scene">
    <dealer ref="dealer" @now="now" @result="end" :tramp="tramp"/>
    <div class="central">
      <div class="status">
        <p>あなた：{{plResult}}</p>
      </div>
      <div class="status">
        <p>相　手：{{dlResult}}</p>
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
      /*
       * 【問題】
       * ここでは勝敗を判定しているよ
       * plResultにはプレーヤーが今まで引いたカードの数が、
       * dlResultには相手が今まで引いたカードの数が、記録されているよ！
       * ２１に近い方が勝ちとなるように、判定条件を加えてみよう！
       */ 
      if (this.isButton) return ''
      else return 'あなたの勝ちです'
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
  margin: 5%;
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
  font-weight: 700;
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