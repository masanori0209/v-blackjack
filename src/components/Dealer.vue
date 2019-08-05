<template>
  <!-- 相手側コンポーネント -->
  <div class="dealer">
    <div class="field">
      <card v-for="(card, index) in hand"
            :key="index"
            :suit="card.mark"
            :number="card.value"
            :hide="card.hide">
      </card>
    </div>
  </div>
</template>

<script>
import Card from './Card'

export default {
  name: 'dealer',
  components: { Card },
  props: ['tramp'],
  data () {
    return {
      hand: [],
      deck: this.tramp
    }
  },
  created: function () {
    this.hand.push(this.pick())
    this.hand.push(this.pick())
    this.hand[0].hide = true
    this.$on('draw', this.draw)
  },
  methods: {
    draw (playerBust) {
      this.hand[0].hide = false
      /*
       * 【問題】
       * 下記ではループ処理をやっているよ！
       * playerBustはプレーヤー側が２１を超えたかどうかのフラグを持っているよ
       * cpuCardMaxではカードを引くのをやめる値が入っているよ
       * 自由に値を変えてみたり、判定条件を変えてみてCPUのレベルを変化させてみよう！
       */
      var cpuCardMax = 17
      // CPUのループ
      while (playerBust == false && this.calc(this.hand) < cpuCardMax) {
        this.hand.push(this.pick())
      }
      this.$emit('result', this.calc(this.hand))
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
      // デッキを一枚引く
      return this.deck.splice(Math.floor(Math.random() * Math.floor(this.deck.length)), 1)[0]
    }
  }
}
</script>
<style scoped>
.field {
  display: -webkit-flex;
  display: -moz-flex;
  display: -ms-flex;
  display: -o-flex;
  display: flex;
  justify-content: center;
}
</style>
