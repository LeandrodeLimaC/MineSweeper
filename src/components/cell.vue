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
      <svg
        v-if="row_item.isMine"
        xmlns="http://www.w3.org/2000/svg"
        xmlns:xlink="http://www.w3.org/1999/xlink"
        aria-hidden="true"
        focusable="false"
        width="1em"
        height="1em"
        style="
          -ms-transform: rotate(360deg);
          -webkit-transform: rotate(360deg);
          transform: rotate(360deg);
        "
        preserveAspectRatio="xMidYMid meet"
        viewBox="0 0 24 24"
      >
        <path
          d="M11.25 6a3.25 3.25 0 0 1 3.25-3.25A3.25 3.25 0 0 1 17.75 6c0 .42.33.75.75.75s.75-.33.75-.75v-.75h1.5V6a2.25 2.25 0 0 1-2.25 2.25A2.25 2.25 0 0 1 16.25 6a1.75 1.75 0 0 0-1.75-1.75A1.75 1.75 0 0 0 12.75 6H14v1.29c2.89.86 5 3.54 5 6.71a7 7 0 0 1-7 7a7 7 0 0 1-7-7c0-3.17 2.11-5.85 5-6.71V6h1.25M22 6h2v1h-2V6m-3-2V2h1v2h-1m1.91.38l1.42-1.42l.71.71l-1.42 1.42l-.71-.71z"
          fill="#000"
        />
      </svg>
      <span v-else>{{ row_item.nearMines }}</span>
    </div>
  </div>
</template>

<script>
// import { mdiBomb } from "@mdi/js";

export default {
  // components: {
  //   mdiBomb,
  // },
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