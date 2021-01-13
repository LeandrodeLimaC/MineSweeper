<template>
  <div id="app">
    <table>
      <tr v-for="(column_item, column_index) in grid" :key="column_index">
        <td
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
    };
  },
  created() {
    this.grid = this.makeGrid();
    this.setMines();
  },
  methods: {
    makeGrid() {
      let array = new Array(this.totalRows);

      for (let i = 0; i < array.length; i++) {
        array[i] = new Array(this.totalColumns);
      }

      return array;
    },
    setMines() {
      this.grid.map((column, column_index) => {
        for (let j = 0; j < column.length; j++) {
          this.$set(this.grid[column_index], j, {
            isMine: Boolean(Math.round(Math.random())),
            isRevealed: false,
          });
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

      this.$set(
        this.grid[column_index][row_index],
        "nearMines",
        this.countNearMines(column_index, row_index)
      );

      this.$set(this.grid[column_index][row_index], "isRevealed", true);
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
