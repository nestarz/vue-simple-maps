<template>
  <g>
    <defs>
      <clipPath :id="id">
        <path :d="spherePath" />
      </clipPath>
    </defs>
    <path
      :d="spherePath"
      :fill="fill"
      :stroke="stroke"
      :stroke-width="strokeWidth"
      class="rsm-sphere"
      v-bind="$attrs"
    />
  </g>
</template>

<script>
import { computed, inject } from "@vue/composition-api";
import { ContextSymbol } from "./utils";

export default {
  inheritAttrs: false,
  props: {
    id: { type: String, default: "rsm-sphere" },
    fill: { type: String, default: "transparent" },
    stroke: { type: String, default: "currentcolor" },
    strokeWidth: { type: Number, default: 0.5 }
  },
  setup() {
    const context = inject(ContextSymbol);

    return {
      spherePath: computed(() => context.path({ type: "Sphere" }))
    };
  }
};
</script>

<style scoped>
.rsm-sphere {
  pointer-events: none;
}
</style>
