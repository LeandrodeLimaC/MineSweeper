<template>
  <div id="app">
    <table>
      <tr v-for="(column_item, column_index) in grid" :key="column_index">
        <td
          style="margin: 0; padding: 0"
          class="cel"
          v-for="(row_item, row_index) in column_item"
          :key="row_index"
        >
          <cell
            :column_index="column_index"
            :row_index="row_index"
            :row_item="row_item"
            @on-cell-revealed="handleCellEvent"
          />
        </td>
      </tr>
    </table>
  </div>
</template>

<script>
import cell from "@/components/cell";

export default {
  components: {
    cell,
  },
  data: () => {
    return {
      totalRows: 10,
      totalColumns: 10,
      grid: [],
      totalMines: 20,
    };
  },
  created() {
    this.grid = this.makeGrid();
    this.setMines();
  },
  methods: {
    makeGrid() {
      let array = new Array(this.totalColumns);

      for (let i = 0; i < array.length; i++) {
        let a = Array(this.totalRows);
        let j = 0;

        while (j < this.totalRows) a[j++] = { isRevealed: false };
        array[i] = a;
      }

      return array;
    },

    setMines() {
      for (let n = 0; n < this.totalMines; n++) {
        let i = Math.round(Math.random() * (this.totalColumns - 1));
        let j = Math.round(Math.random() * (this.totalRows - 1));

        this.$set(this.grid[i][j], "isMine", true);
      }
    },

    countNearMines(column_index, row_index) {
      let nearMines = 0;

      for (let xoff = -1; xoff <= 1; xoff++) {
        for (let yoff = -1; yoff <= 1; yoff++) {
          let i = column_index + yoff;
          let j = row_index + xoff;

          if (i > -1 && i < this.totalColumns && j > -1 && j < this.totalRows) {
            let neighbor = this.grid[i][j];

            if (neighbor.isMine) nearMines++;
          }
        }
      }

      return nearMines;
    },

    handleCellEvent(event) {
      const { column_index, row_index } = event;
      let cell = this.grid[column_index][row_index];

      this.$set(
        cell,
        "nearMines",
        this.countNearMines(column_index, row_index)
      );
      this.$set(cell, "isRevealed", true);

      if (!cell.isMine && !cell.nearMines) {
        this.loopNeighborCells({ column_index, row_index }, this.floodFill);
      }
    },

    loopNeighborCells({ column_index, row_index }, paramFunc) {
      for (let xoff = -1; xoff <= 1; xoff++) {
        for (let yoff = -1; yoff <= 1; yoff++) {
          let i = column_index + yoff;
          let j = row_index + xoff;

          if (i > -1 && i < this.totalColumns && j > -1 && j < this.totalRows) {
            paramFunc(i, j);
          }
        }
      }
    },

    floodFill(column_index, row_index) {
      let neighbor = this.grid[column_index][row_index];

      if (!neighbor.isMine && !neighbor.isRevealed) {
        this.$set(neighbor, "isRevealed", true);

        this.handleCellEvent({ column_index, row_index });
      }
    },
  },
};
</script>

<style scoped>
.cel {
  font-size: 14px;
  width: 1rem;
  height: 1rem;
  text-align: center;
  border: 1px solid black;
}
table {
  border-collapse: collapse;
}
</style>
