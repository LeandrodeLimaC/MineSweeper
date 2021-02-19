<template>
  <div @click="handleClick" style="height: 100%; width: 100%">
    <div
      v-show="row_item.isRevealed"
      style="
        background: #e1e1e1;
        display: flex;
        justify-content: center;
        align-items: center;
        height: 100%;
        width: 100%;
      "
    >
      <img v-if="row_item.isMine" :src="mine_svg" />
      <span v-else>{{ row_item.nearMines }}</span>
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
    mine_svg: () => require("@/assets/bomb.svg"),
  },
  methods: {
    handleClick: function () {
      if (this.isRevealed) return;

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