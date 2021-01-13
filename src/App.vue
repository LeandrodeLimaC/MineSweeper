<template>
  <div id="app">
    <table>
      <tr v-for="(column_item, column_index) in grid" :key="column_index">
        <td
          class="cel"
          v-for="(row_item, row_index) in column_item"
          :key="row_index"
          @click="a(column_index, row_index)"
        >
          {{ row_item }}
          <!-- <cell
            :column="column_index"
            :row="row_index"
            @on-cell-revealed="handleCellEvent"
          /> -->
        </td>
      </tr>
    </table>
  </div>
</template>

<script>
// import cell from "@/components/cell";

export default {
  components: {
    // cell,
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
    console.log(this.grid);
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
      for (let i = 0; i < this.grid.length; i++) {
        for (let j = 0; j < this.grid[0].length; j++) {
          this.$set(this.grid[i], j, { isMine: false, isRevealed: false });
        }
      }
    },
    a(column_index, row_index) {
      this.$set(this.grid[column_index][row_index], "isRevealed", true);
    },
    handleCellEvent(event) {
      console.log(event);
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
