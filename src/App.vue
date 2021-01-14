<template>
  <div id="app">
    <table>
      <tr v-for="(column_item, column_index) in grid" :key="column_index">
        <td
          class="cel"
          v-for="(row_item, row_index) in column_item"
          :key="row_index"
        >
          <!-- {{ row_item }} -->
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
      totalMines: 10,
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
        let a = Array(this.totalRows),
          j = 0;

        while (j < this.totalRows) a[j++] = { isRevealed: false };
        array[i] = a;
      }

      return array;
    },
    setMines() {
      this.grid.map((column, column_index) => {
        for (let j = 0; j < column.length; j++) {
          let boolean;

          if (Boolean(Math.round(Math.random())) && this.totalMines) {
            boolean = true;
            this.totalMines--;
          } else {
            boolean = false;
          }

          this.$set(this.grid[column_index][j], "isMine", boolean);
        }
      });
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
        this.floodFill(column_index, row_index);
      }
    },

    floodFill(column_index, row_index) {
      for (let xoff = -1; xoff <= 1; xoff++) {
        for (let yoff = -1; yoff <= 1; yoff++) {
          let i = column_index + yoff;
          let j = row_index + xoff;

          if (i > -1 && i < this.totalColumns && j > -1 && j < this.totalRows) {
            let neighbor = this.grid[i][j];

            if (!neighbor.isMine && !neighbor.isRevealed) {
              this.$set(neighbor, "isRevealed", true);

              this.handleCellEvent({ column_index: i, row_index: j });
            }
          }
        }
      }
    },
  },
};
</script>

<style scoped>
table {
  border-collapse: collapse;
}
.cel {
  width: 1rem;
  height: 1rem;
  text-align: center;
  border: 1px solid black;
}
</style>
