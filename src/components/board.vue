<template>
  <table cellpadding="0" cellspacing="0">
    <tr v-for="(row, row_index) in grid" :key="row_index">
      <cell
        v-for="(tile, tile_index) in row"
        :tile="tile"
        @on-cell-revealed="revealCell"
        :key="tile_index"
      />
    </tr>
  </table>
</template>

<script>
import cell from "@/components/cell";

export default {
  name: "board",
  components: { cell },
  props: {
    max_rows: Number,
    max_columns: Number,
    gameStats: {
      type: Object,
      required: true,
    },
    grid: {
      type: Array,
      required: true,
    },
  },
  data() {
    return {
      flagMode: false,
      mines: {
        found: 0,
      },
    };
  },
  methods: {
    revealCell(tile) {
      if (this.gameStats.isGameOver) return;
      !tile.isMine || this.gameOver();

      if (this.flagMode) {
        return (tile.flagged = true);
      }

      let neighbors = this.getNeigbors(tile.row, tile.column);

      tile.isRevealed = true;
      tile.nearMines = this.countNearMines(neighbors);

      if (!tile.isMine && !tile.nearMines) {
        this.floodFill(neighbors);
      }
    },

    gameOver() {
      this.$emit("on-game-over");

      this.revealAllMines();
    },

    revealAllMines() {
      this.grid.forEach((col) => {
        col.forEach((tile) => {
          !tile.isMine || (tile.isRevealed = true);
        });
      });
    },

    getNeigbors(row_index, column_index) {
      let neighbors = [];

      for (let xoff = -1; xoff <= 1; xoff++) {
        for (let yoff = -1; yoff <= 1; yoff++) {
          let i = row_index + xoff;
          let j = column_index + yoff;

          if (i > -1 && i < this.max_rows && j > -1 && j < this.max_columns) {
            neighbors.push(this.grid[i][j]);
          }
        }
      }

      return neighbors;
    },

    countNearMines(neighbors) {
      let nearMines = 0;

      neighbors.forEach((tile) => {
        if (tile.isMine) nearMines++;
      });

      return nearMines;
    },

    floodFill(neighbors) {
      neighbors.forEach((tile) => {
        if (!tile.isMine && !tile.isRevealed) {
          tile.flagged || this.revealCell(tile);
        }
      });
    },
  },
};
</script>

<style>
</style>
