<template>
  <div class="board">
    <div class="inner">
      <div
        class="row"
        v-for="(row, rowIdx) in board"
        :key="rowIdx"
      >
        <div
          class="cell"
          v-for="(cell, cellIdx) in board[rowIdx]"
          :key="cellIdx"
        >
          <Cell :value="cell" :x="cellIdx" :y="rowIdx" @turn="handleTurn" />
        </div>
      </div>
    </div>

    <pre>
      {{ boardValues() }}
    </pre>
  </div>
</template>

<script>
import Cell from './Cell'

export default {
  name: 'Board',

  components: { Cell },

  data: () => ({
    board: [
      [0,0,0,0,0],
      [0,0,0,0,0],
      [0,0,0,0,0],
      [0,0,0,0,0],
      [0,0,0,0,0],
    ],
    wins: [
      [[0, 0], [0, 1], [0, 2], [0, 3]], // 0
      [[0, 1], [0, 2], [0, 3], [0, 4]], // 1
      [[1, 0], [1, 1], [1, 2], [1, 3]], // 2
      [[1, 1], [1, 2], [1, 3], [1, 4]], // 3
      [[2, 0], [2, 1], [2, 2], [2, 3]], // 4
      [[2, 1], [2, 2], [2, 3], [2, 4]], // 5
      [[3, 0], [3, 1], [3, 2], [3, 3]], // 6
      [[3, 1], [3, 2], [3, 3], [3, 4]], // 7
      [[4, 0], [4, 1], [4, 2], [4, 3]], // 8
      [[4, 1], [4, 2], [4, 3], [4, 4]], // 9
      [[0, 0], [1, 0], [2, 0], [3, 0]], // 10
      [[1, 0], [2, 0], [3, 0], [4, 0]], // 11
      [[0, 1], [1, 1], [2, 1], [3, 1]], // 12
      [[1, 1], [2, 1], [3, 1], [4, 1]], // 13
      [[0, 2], [1, 2], [2, 2], [3, 2]], // 14
      [[1, 2], [2, 2], [3, 2], [4, 2]], // 15
      [[0, 3], [1, 3], [2, 3], [3, 3]], // 16
      [[1, 3], [2, 3], [3, 3], [4, 3]], // 17
      [[0, 4], [1, 4], [2, 4], [3, 4]], // 18
      [[1, 4], [2, 4], [3, 4], [4, 4]], // 19
      [[0, 0], [1, 1], [2, 2], [3, 3]], // 20
      [[1, 1], [2, 2], [3, 3], [4, 4]], // 21
      [[0, 1], [1, 2], [2, 3], [3, 4]], // 22
      [[1, 0], [2, 1], [3, 2], [4, 3]], // 23
      [[4, 0], [3, 1], [2, 2], [1, 3]], // 24
      [[3, 1], [2, 2], [1, 3], [0, 4]], // 25
      [[0, 3], [1, 2], [2, 1], [3, 0]], // 26
      [[1, 4], [2, 3], [3, 2], [4, 1]], // 27
    ],
    yourTurn: true,
  }),

  // computed: {},

  watch: {
    yourTurn (value) {
      this.$forceUpdate()
      if (!value) {
        this.makeAITurn()
      }
    }
  },

  methods: {
    handleTurn ({ x, y, value }) {
      if (!this.yourTurn || value > 0) {
        return true
      }
      this.board[y][x] = 1
      this.yourTurn = false
    },

    makeAITurn () {
      const filled = new Map(this.boardValues()
        .map((value, idx) => [idx, { value, cells: this.wins[idx] }]))

      console.log(filled)
    },

    boardValues () {
      return this.wins.map((winLine) => 
        winLine.map(([x, y]) => this.board[y][x])
      )
    },
  },
}
</script>

<style lang="stylus">
.board
  display flex
  flex-direction column
  align-items center

  .inner
    border-top 1px solid
    border-right 1px solid

    .row
      display flex

      .cell
        cursor pointer
        border-left 1px solid
        border-bottom 1px solid
        width 30px
        height 30px
</style>
