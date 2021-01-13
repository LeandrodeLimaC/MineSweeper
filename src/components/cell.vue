<template>
  <div @click="handleClick" style="height: 100%; width: 100%">
    <div v-show="row_item.isRevealed" style="height: 100%; width: 100%">
      <span v-if="row_item.isMine">M</span>
      <span v-else>0</span>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    column_index: Number,
    row_index: Number,
    row_item: {},
  },
  computed: {
    mine: () => Boolean(Math.round(Math.random())),
  },
  methods: {
    handleClick: function () {
      if (this.isRevealed) return;

      console.log(
        `Posição : ${this.column_index}${this.row_index}, é uma mina: ${this.mine}`
      );
      // if (this.mine)
      //   return this.$emit("on-cell-revealed", "Olá, sou uma mina, se fudeu");

      this.$emit("on-cell-revealed", {
        column_index: this.column_index,
        row_index: this.row_index,
      });
    },
  },
};
</script>

<style>
</style>