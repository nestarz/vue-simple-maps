<template>
  <g :transform="translate" class="rsm-annotation">
    <path :d="connectorPath" fill="transparent" v-bind="$attrs" />
    <slot />
  </g>
</template>

<script>
import { createConnectorPath } from "./utils";
import { computed, inject } from "@vue/composition-api";
import { ContextSymbol } from "./utils";

export default {
  inheritAttrs: false,
  props: {
    subject: Array,
    dx: { type: Number, default: 30 },
    dy: { type: Number, default: 30 },
    curve: { type: Number, default: 0 },
    connectorProps: { type: Object, default: () => ({ stroke: "#000" }) }
  },
  setup(props) {
    const context = inject(ContextSymbol);
    const point = computed(() => {
      context.update;
      if (!context.projection) return { x: 0, y: 0 };

      const [x, y] = context.projection(props.subject);
      return { x, y };
    });
    
    return {
      translate: computed(
        () =>
          `translate(${point.value.x + props.dx}, ${point.value.y + props.dy})`
      ),
      connectorPath: computed(() =>
        createConnectorPath(props.dx, props.dy, props.curve)
      )
    };
  }
};
</script>