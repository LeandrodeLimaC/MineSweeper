<template>
  <td @click="handleClick">
    <div v-show="tile.isRevealed" class="cell_revealed">
      <img v-if="tile.isMine" :src="mine_svg" />
      <span v-else>{{ tile.nearMines }}</span>
    </div>
  </td>
</template>

<script>
export default {
  name: "Cell",
  props: {
    tile: {
      type: Object,
      require: true,
    },
  },
  computed: {
    mine_svg: () => require("@/assets/bomb.svg"),
  },
  methods: {
    handleClick: function () {
      if (this.isRevealed) return;

      this.$emit("on-cell-revealed", this.tile);
    },
  },
};
</script>

<style>
td {
  font-size: 14px;
  width: 1rem;
  height: 1rem;
  text-align: center;
  border: 1px solid black;
}
.cell_revealed {
  background: #e1e1e1;
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100%;
  width: 100%;
}
</style>