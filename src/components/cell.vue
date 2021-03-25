<template>
  <td @click="handleClick">
    <span v-if="tile.flagged">{{ tile }}</span>
    <div v-show="tile.isRevealed" class="cell_revealed">
      <icon-base
        width="26"
        height="26"
        v-if="tile.isMine"
        icon-name="moon"
        iconColor="#3b1f67"
      >
        <icon-bomb />
      </icon-base>
      <span v-else>{{ tile.nearMines || null }}</span>
    </div>
  </td>
</template>

<script>
import iconBomb from "@/components/icons/iconBomb";
import iconBase from "@/components/iconBase";

export default {
  name: "Cell",
  props: {
    tile: {
      type: Object,
      require: true,
    },
  },
  components: {
    iconBase,
    iconBomb,
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
  background: #a679ea;
  margin: 2px;
  border-radius: 50px;
  display: inline-block;
  overflow: hidden;
  font-size: 25px;
  width: 3rem;
  height: 3rem;
  text-align: center;
}
.cell_revealed {
  background: #ffffffcf;
  color: #a679ea;
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100%;
  width: 100%;
}
</style>