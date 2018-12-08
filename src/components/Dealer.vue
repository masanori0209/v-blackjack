<template>
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
      while (!playerBust && this.calc(this.hand) < 17) {
        this.hand.push(this.pick())
      }
      this.$emit('result', this.calc(this.hand))
    },
    calc (hand) {
      const points = hand.map(card => (card.value > 10 ? 10 : card.value))
      const sum = points.reduce((ret, cur) => ret + cur)
      if (sum > 21) {
        return 'Bust (' + sum + ')'
      }
      return (sum <= 11 && points === 1) ? (sum + 10) : sum
    },
    pick () {
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
