<template>
  <svg
    width="400"
    height="300">
    <g>
      <polyline
        :stroke="stroke"
        points="0,0 400,300"/>
      <polyline
        :stroke="stroke"
        points="0,300 400,0"/>
    </g>
  </svg>
</template>
<script>
export default {
  props: {
    stroke: {
      type: String,
      default: '#000'
    },
    values: {
      type: Array,
      default () {
        return [1, 2, 3]
      }
    },
    minValue: {
      type: Number,
      default: 0
    }
  }
}
</script>
<style scoped lang="scss">
svg {
  display: block;

  polyline {
    fill: none;
  }
}
</style>
