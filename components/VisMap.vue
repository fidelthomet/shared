<template>
  <svg
    width="600"
    height="300">
    <g>
      <polyline points="0,0 300,300 600,0"/>
      <polyline points="0,300 300,0 600,300"/>
    </g>
  </svg>
</template>
<style scoped lang="scss">
@import "../assets/style/global";
svg {
  display: block;

  polyline {
    fill: none;
    stroke: $color-violet;
  }
}
</style>
