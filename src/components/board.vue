<template>
  <table>
    <tr v-for="(row, row_index) in grid" :key="row_index">
      <cell
        v-for="(tile, tile_index) in row"
        :tile="tile"
        @on-cell-revealed="handleCellEvent"
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
    max_mines: Number,
  },
  data: () => {
    return {
      grid: [],
    };
  },
  created() {
    this.makeGrid();
    this.setMines();
  },
  methods: {
    makeGrid() {
      this.grid = new Array(this.max_rows).fill(0).map((row, rowIndex) => {
        return new Array(this.max_columns)
          .fill(0)
          .map((column, columnIndex) => {
            return {
              row: rowIndex,
              column: columnIndex,
              flagged: false,
              isRevealed: false,
            };
          });
      });
    },

    setMines() {
      for (let n = 0; n < this.max_mines; n++) {
        let i = Math.round(Math.random() * (this.max_rows - 1));
        let j = Math.round(Math.random() * (this.max_columns - 1));

        this.$set(this.grid[i][j], "isMine", true);
      }
    },

    handleCellEvent(tile) {
      let neighbors = this.getNeigbors(tile.row, tile.column);

      tile.isRevealed = true;
      tile.nearMines = this.countNearMines(neighbors);

      if (!tile.isMine && !tile.nearMines) {
        this.floodFill(neighbors);
      }
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
          this.handleCellEvent(tile);
        }
      });
    },
  },
};
</script>

<style scoped>
table {
  border-collapse: collapse;
}
</style>
