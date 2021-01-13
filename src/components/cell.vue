<template>
  <div @click="handleClick" style="height: 100%; width: 100%">
    <div v-show="isRevealed" style="height: 100%; width: 100%">
      <span v-if="mine">M</span>
      <span v-else>0</span>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    column: Number,
    row: Number,
  },
  data: () => {
    return {
      isRevealed: false,
    };
  },
  computed: {
    mine: () => Boolean(Math.round(Math.random())),
  },
  methods: {
    handleClick: function () {
      if (this.isRevealed) return;

      this.isRevealed = true;
      // console.log(
      //   `Posição : ${this.column}${this.row}, é uma mina: ${this.mine}`
      // );
      if (this.mine)
        return this.$emit("on-cell-revealed", "Olá, sou uma mina, se fudeu");

      this.$emit("on-cell-revealed", "Olá, não sou uma mina, tamujunto");
    },
  },
};
</script>

<style>
</style>