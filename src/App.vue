<template>
  <div id="app">
    <board
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
      config: {
        maxColumns: 10,
        maxRows: 10,
        maxMines: 30,
      },
    };
  },
};
</script>

<style scoped>
table {
  border-collapse: collapse;
}
</style>
