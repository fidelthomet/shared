<template>
  <section
    class="container"
    v-html="html"/>
</template>

<script>
import markdownIt from 'markdown-it'
import README from '../README.md'
// import EventEmitter from '~/components/EventEmitter.vue'
// import Glossary from '~/components/Glossary.vue'

const md = markdownIt({
  html: true,
  typographer: true
})
export default {
  data () {
    return {
      html: md.render(README.replace(/.*\n/, ''))
    }
  }
}
</script>

<style>
</style>
