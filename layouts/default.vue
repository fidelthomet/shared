<template>
  <div>
    <header>
      <h1>
        <strong>SENSES </strong>
        <nuxt-link to="/">Style</nuxt-link> and <nuxt-link to="/components">Components</nuxt-link>
      </h1>
      <h1><nuxt-link to="/docs">Docs</nuxt-link></h1>
    </header>
    <nuxt/>
  </div>
</template>

<style scoped lang="scss">
@import "~@/assets/style/global";
header {
  margin: $spacing-unit / 2;
  @include flex-row;
  justify-content: space-between;
}
</style>
