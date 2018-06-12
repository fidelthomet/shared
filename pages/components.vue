<template>
  <section class="container">
    <div
      v-for="component in components"
      :key="component.name">
      <h2>{{ component.name }}</h2>
      <span class="tags">
        <code
          v-for="tag in component.tags"
          :key="`${component.name}-${tag}`">{{ tag }}</code>
      </span>
      <br>
      <div class="component-outer">
        <div class="component-inner">
          <component :is="component.name"/>
        </div>
      </div>
      <div class="meta">
        <div class="code">
          <pre>{{ component.markupShort }}</pre>
          <pre>{{ component.markup }}</pre>
        </div>
        <div class="props">
          <pre>{{ JSON.stringify(component.props, null, 2) }}</pre>
        </div>
      </div>
    </div>
  </section>
</template>

<script>
import components from '~/components'

export default {
  components: {
    ...components
  },
  computed: {
    components () {
      return Object.keys(components).map(name => {
        const component = components[name]
        const props = component.props
        const markupProps = props ? Object.keys(props).map(propName => `${propName}="${props[propName].default}"`).join('\n  ') : null
        console.log(markupProps)
        return {
          name,
          tags: component.tags,
          props: component.props,
          markupShort: `<${name}/>`,
          markup: `<${name}\n  ${markupProps}/>`
        }
      })
    }
  },
  created () {
    console.log(this.components)
  }
}
</script>

<style scoped lang="scss">
@import "~@/assets/style/global";

.tags {
  code {
    margin-right: $spacing-unit * 0.25;
  }
}

.component-outer {
  background: url('~/assets/img/pattern.svg') repeat;
  padding: $spacing-unit / 2;
  margin: #{$spacing-unit / 2} #{-$spacing-unit / 2};

  .component-inner {
    background: $color-white;
    display: inline-block;
    vertical-align: top;
  }
}

.meta {
  @include flex-row;
  align-items: flex-start;
  flex-wrap: wrap;
  margin: 0 #{-$spacing-unit / 4};

  .props, .code {
    flex: auto;
    margin: -$spacing-unit / 2 $spacing-unit / 4;

    pre {
      margin-top: $spacing-unit / 2;
    }
  }
}
</style>
