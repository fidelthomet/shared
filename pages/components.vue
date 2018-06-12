<template>
  <section class="container">
    <resize-observer @notify="setMaxWidth()" />
    <div class="tags filter">
      <span
        :class="{active: filter.length === 0}"
        class="tag"
        @click="filter = []">All</span>
      <span
        v-for="tag in tags"
        :key="`${tag}`"
        :class="{active: filter.find(f => f === tag)}"
        class="tag"
        @click="toggleFilter(tag)">{{ tag }}</span>
    </div>
    <div
      v-for="component in components"
      :key="component.name">
      <h2>{{ component.name }}</h2>
      <span class="tags">
        <span
          v-for="tag in component.tags"
          :key="`${component.name}-${tag}`"
          :class="{active: filter.find(f => f === tag)}"
          class="tag"
          @click="toggleFilter(tag)">{{ tag }}</span>
      </span>
      <br>
      <div class="component-outer">
        <div
          :style="{'max-width': `${maxWidth}px`}"
          class="component-inner">
          <component :is="component.name"/>
        </div>
      </div>
      <div class="meta">
        <div class="code">
          <pre>{{ component.markupShort }}</pre>
          <pre v-if="component.props">{{ component.markup }}</pre>
        </div>
        <div
          v-if="component.props"
          class="props">
          <table>
            <thead>
              <tr>
                <th>Property</th>
                <th>Type</th>
                <th>Default</th>
              </tr>
            </thead>
            <tbody>
              <tr
                v-for="prop in component.props"
                :key="`${component.name}-${prop.name}`">
                <td>{{ prop.name }}</td>
                <td>{{ prop.type }}</td>
                <td>{{ prop.default }}</td>
              </tr>
            </tbody>
          </table>
        </div>
      </div>
    </div>
    <div class="max-width">
      max-width: {{ maxWidth }}px
      <input
        ref="maxWidth"
        v-model="maxWidth"
        :max="maxMaxWidth"
        type="range"
        min="0">
    </div>
  </section>
</template>

<script>
import components from '~/components'
import { ResizeObserver } from 'vue-resize'

export default {
  components: {
    ...components,
    ResizeObserver
  },
  data () {
    return {
      filter: [],
      maxWidth: Infinity,
      maxMaxWidth: Infinity
    }
  },
  computed: {
    components () {
      return Object.keys(components).map(name => {
        const component = components[name]
        let props = null
        let markup = null
        if (component.props) {
          props = Object.keys(component.props).map(propName => {
            const prop = component.props[propName]
            return {
              name: propName,
              type: prop.type.name,
              default: typeof prop.default === 'function' ? JSON.stringify(prop.default()) : prop.default
            }
          })
          const propsString = props.map(p => {
            if (p.type === 'Object' || p.type === 'Array') return `:${p.name}="[${p.type}]"`
            if (p.type === 'String') return `${p.name}="${p.default}"`
            return `:${p.name}="${p.default}"`
          }).join('\n  ')
          markup = `<${name}${props.length > 1 ? '\n  ' : ' '}${propsString}/>`
        }
        return {
          name,
          tags: component.tags,
          props,
          markupShort: `<${name}/>`,
          markup
        }
      }).filter(c => [...new Set([...this.filter, ...c.tags])].length === c.tags.length)
    },
    tags () {
      return [...new Set(this.components.map(c => c.tags).reduce((a, b) => a.concat(b), []))]
    }
  },
  methods: {
    toggleFilter (tag) {
      if (this.filter.find(f => f === tag)) {
        this.filter = this.filter.filter(f => f !== tag)
      } else {
        this.filter.push(tag)
      }
    },
    setMaxWidth () {
      const maxMaxWidth = this.$refs.maxWidth.getBoundingClientRect().width
      if (this.maxMaxWidth <= this.maxWidth) {
        this.maxWidth = maxMaxWidth
      }
      this.maxMaxWidth = maxMaxWidth
    }
  }
}
</script>

<style scoped lang="scss">
@import "~@/assets/style/global";

.filter {
  margin-bottom: $spacing-unit;
}

h2 {
  display: inline-block;
  margin-right: $spacing-unit * 0.25;
}

.tags {
  .tag {
    margin-right: $spacing-unit * 0.25;
    background: $color-pale-gray;
    padding: 0 #{$spacing-unit / 8};

    &:hover, &.active {
      background: $color-violet;
      color: $color-white;
      cursor: default;
    }
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
    overflow: scroll;
  }
}

.meta {
  @include flex-row;
  align-items: flex-start;
  flex-wrap: wrap;
  margin: 0 #{-$spacing-unit / 4} $spacing-unit;

  .props, .code {
    flex: auto;
    margin: 0 $spacing-unit / 4;

    pre {
      margin-bottom: $spacing-unit / 2;
    }
  }
}

.max-width {
  position: fixed;
  background: $color-violet;
  color: $color-white;
  left: 0;
  bottom: 0;
  width: 100%;
  padding: $spacing-unit / 4 $spacing-unit / 2;

  input[type="range"] {
    display: block;
    width: 100%;
  }
}
</style>
