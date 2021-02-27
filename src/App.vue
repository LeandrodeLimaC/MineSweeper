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
      :max_mines="config.maxMines"
    />
  </div>
</template>

<script>
import board from "./components/board";

export default {
  components: { board },
  data: () => {
    return {
      gameStats: {
        isGameOver: false,
      },
      config: {
        maxColumns: 10,
        maxRows: 10,
        maxMines: 30,
      },
    };
  },
  methods: {
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
}
table {
  border-collapse: collapse;
}
</style>
