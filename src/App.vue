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
            :column="column_index"
            :row="row_index"
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
    };
  },
  computed: {
    grid: function () {
      let array = new Array(this.totalRows);

      for (let i = 0; i < array.length; i++) {
        array[i] = new Array(this.totalColumns);
      }

      return array;
    },
  },
  methods: {
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
