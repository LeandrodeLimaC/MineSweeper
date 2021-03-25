<template>
  <div id="app">
    <h1>
      <span v-show="!gameStats.isGameOver">Playing</span>
      <span v-show="gameStats.isGameOver">Game Over</span>
    </h1>
    <board
      @on-game-over="handleGameOver"
      :gameStats="gameStats"
      :max_rows="config.maxRows"
      :max_columns="config.maxColumns"
      :grid="grid"
    />
  </div>
</template>

<script>
import board from "./components/board";

export default {
  components: { board },
  data: () => {
    return {
      grid: [],
      gameStats: {
        isGameOver: false,
      },
      config: {
        maxColumns: 10,
        maxRows: 10,
        difficulty: 1,
      },
    };
  },
  created() {
    this.gameInit();
  },
  methods: {
    gameInit() {
      this.makeGrid();
      // this.setMines(this.grid);
    },

    makeGrid() {
      const { maxRows, maxColumns } = this.config;

      this.grid = new Array(maxRows).fill(0).map((row, rowIndex) => {
        return new Array(maxColumns).fill(0).map((column, columnIndex) => {
          return {
            row: rowIndex,
            column: columnIndex,
            flagged: false,
            isRevealed: false,
            isMine: this.setMines(),
          };
        });
      });
    },

    // https://github.com/chrisnorwood/vue-minesweeper/blob/master/src/App.vue
    setMines() {
      let diffToProb = [
        [10, 9],
        [8, 7],
        [6, 5],
      ];

      let timesFactor = diffToProb[this.config.difficulty][0];
      let compareFactor = diffToProb[this.config.difficulty][1];

      console.log(Math.random() * timesFactor);

      return Math.random() * timesFactor > compareFactor;
      // const { maxMines, maxRows, maxColumns } = this.config;

      // const getPosition = (maxNumber) => {
      //   return Math.round(Math.random() * (maxNumber - 1));
      // };

      // let possibilities = [...grid];

      // possibilities.forEach((row) => {
      //   const column = getPosition(maxColumns);

      //   if (possibilities[row][column]) {
      //     console.log(possibilities);
      //     this.$set(this.grid[row][column], "isMine", true);
      //     possibilities[row].splice([column] - 1, 1);
      //     //
      //   }
      // });
      // console.log(possibilities);
      // for (let n = 0; n < maxMines; n++) {
      //   const row = getPosition(maxRows);
      //   const column = getPosition(maxColumns);

      //   if (possibilities[row][column]) {
      //     console.log(possibilities);
      //     this.$set(this.grid[row][column], "isMine", true);
      //     possibilities[row].splice([column] - 1, 1);
      //     //
      //   }
      // }
    },

    handleGameOver() {
      this.gameStats.isGameOver = true;
    },
  },
};
</script>

<style>
body {
  display: flex;
  justify-content: center;
}
h1 {
  text-align: center;
  font-size: 3rem;
}
table {
  border-collapse: collapse;
}
</style>
