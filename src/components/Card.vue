<template>
  <div class="card tramp" :class="{ 'is-hide': hide }">
    <img :src="require('../assets/' + suit + '.png')" v-if="!hide">
    <p class="left">{{card}}</p><p class="right">{{card}}</p>
  </div>
</template>

<script>
export default {
  name: 'card',
  props: {
    number: Number,
    suit: Number,
    hide: Boolean,
  },
  computed: {
    card: function () {
      if (this.number ===  1) return 'A'
      if (this.number === 11) return 'J'
      if (this.number === 12) return 'Q'
      if (this.number === 13) return 'K'
      return this.number
    }
  }
}
</script>
<style scoped>
.tramp {
  border: solid 3px #AAA;
  width: 100px;
  height: 150px;
  border-radius: 10px;
  margin: 15px;
  color: #222;
}
.is-hide {
  background-color: #222;
}
img {
  position: absolute;
  top: 10px;
  right: 0;
  transform: scale(0.8);
}
.left {
  position: absolute;
  top: 3px;
  left: 3px;
  font-weight: 700;
}
.right {
  position: absolute;
  bottom: 3px;
  right: 3px;
  font-size: 15px;
  font-weight: 800;
}
</style>