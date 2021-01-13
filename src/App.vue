<template>
  <div id="app">
    <table>
      <td v-for="(row, index) in grid" :key="index">
        <tr v-for="(column, index) in row" :key="index" v-on:click="test">
          {{
            index
          }}
        </tr>
      </td>
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

<style>
</style>
