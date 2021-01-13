<template>
  <div id="app">
    <table>
      <tr v-for="(column_item, index) in grid" :key="index">
        <td
          class="cel"
          v-for="(row, index) in column_item"
          :key="index"
          v-on:click="test"
        >
          {{ index }}
        </td>
      </tr>
    </table>
  </div>
</template>

<script>
export default {
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
    test: () => {
      console.log("olá");
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
