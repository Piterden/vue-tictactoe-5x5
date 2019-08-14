<template>
  <div class="cell-component" @click="$emit('turn', { x, y, value })">
    <div class="input">{{ display }}</div>
  </div>
</template>

<script>
export default {
  name: 'Cell',

props: {
    value: { type: Number, default: 0 },
    x: { type: Number, required: true },
    y: { type: Number, required: true },
  },

  computed: {
    display () {
      let value
      switch (this.value) {
        case 0: value = ' '; break
        case 1: value = 'X'; break
        case 2: value = '0'; break
        default:
      }
      return value
    }
  },
}
</script>

<style lang="stylus">
.cell-component .input
  height 30px
  line-height 30px
</style>

