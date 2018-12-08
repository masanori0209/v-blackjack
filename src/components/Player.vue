<template>
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
      <button class="button is-info footer-left"  @click="hit">Hit</button>
      <button class="button is-warning footer-right" @click="stand">Stand</button>
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
      var points = hand.map(card => (card.value > 10 ? 10 : card.value))
      var sum = points.reduce((bfr, aft) => bfr + aft)
      if (sum > 21) {
        return 'Bust (' + sum + ')'
      }
      return (sum <= 11 && points === 1) ? (sum + 10) : sum
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
  width: 30vw;
  height: 10vh;
}
.footer-right {
  width: 30vw;
  height: 10vh;
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