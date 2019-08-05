<template>
  <!-- プレーヤー側コンポーネント -->
  <div class="player">
    <div class="field">
      <card v-for="(card, index) in hand"
            :key="index"
            :suit="card.mark"
            :number="card.value"
            :hide="card.hide">
      </card>
    </div>
    <div class="field is-bottom" v-if="isButton">
      <button class="button is-info footer-left"  @click="hit">カードを引く</button>
      <button class="button is-warning footer-right" @click="stand">勝負する！</button>
    </div>
  </div>
</template>

<script>
import Card from './Card'

export default {
  name: 'player',
  components: { Card },
  props: ['isButton', 'tramp'],
  data () {
    return {
      hand: [],
      result: 0,
      deck: this.tramp
    }
  },
  created: function () {
    this.hand.push(this.pick())
    this.hand.push(this.pick())
    this.result = this.calc(this.hand)
  },
  methods: {
    hit () {
      this.hand.push(this.pick())
      this.result = this.calc(this.hand)
    },
    stand () {
      this.$emit('stand', this.result)
    },
    calc (hand) {
      /*
       * 【問題】
       * 計算結果を保持しているよ！
       * このままだとバーストした場合の条件がわかりにくいね。
       * 条件を追加してバーストした場合とそうで無い場合に分けてみよう！
       * それに J, Q, K の値をそのまま出してしまっているよ！
       * J, Q, K の場合は 10 で加算されるように直してみよう！
       * できた人はAの場合の加算変化もやってみよう！
       */
      var sum = 0
      for (var h in this.hand) {
        sum += this.hand[h].value 
      }
      return sum
    },
    pick () {
      return this.deck.splice(Math.floor(Math.random() * Math.floor(this.deck.length)), 1)[0]
    }
  },
  watch: {
    result: function (newValue, oldValue) {
      if (String(newValue).indexOf('Bust') > -1) {
        this.$emit('stand', newValue)
      } else {
        this.$emit('now', newValue, 'player')
      }
    }
  }
}
</script>
<style scoped>
.is-bottom {
  bottom: 0px;
}
.footer-left {
  width: 20vw;
  height: 5vh;
}
.footer-right {
  width: 20vw;
  height: 5vh;
}
.field {
  display: -webkit-flex;
  display: -moz-flex;
  display: -ms-flex;
  display: -o-flex;
  display: flex;
  justify-content: center;
}
</style>