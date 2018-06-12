<template>
  <section class="container">
    <h2>Colors</h2>
    <div
      v-for="color in colors"
      :key="color.name"
      class="color">
      <div
        :style="{background: color.value}"
        class="fill"/>
      <div class="code">
        <code>${{ color.name }}</code>
        <br>
        <code>{{ color.value }}</code>
      </div>
    </div>
  </section>
</template>

<script>
import VisLineChart from '~/components/VisLineChart.vue'
import colors from '~/assets/style/colors.scss' // eslint-disable-line

export default {
  components: {
    VisLineChart
  },
  data () {
    return {
      colors: Object.keys(colors).map(name => ({name, value: colors[name]}))
    }
  }
}
</script>

<style scoped lang="scss">
@import "~@/assets/style/global";

.color {
  @include flex-row;
  align-items: flex-start;
  justify-content: flex-start;
  line-height: 20px;

  .fill {
    width: $spacing-unit * 4;
    height: $spacing-unit * 1.5;
    border: 1px solid $color-pale-gray;
    margin: 0 #{$spacing-unit / 4} #{$spacing-unit / 4} 0;
  }
  .code {
    @include flex-column;
    align-items: flex-start;
    justify-content: space-around;
    height: 48px;
  }
}
</style>
