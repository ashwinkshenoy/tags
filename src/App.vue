<template>
  <div id="app">
    <UrlFetcher />
  </div>
</template>

<script>
import UrlFetcher from './components/UrlFetcher.vue'

export default {
  name: 'app',
  components: {
    UrlFetcher
  },
  meta: {
    title: 'Tags Fetcher'
  }
}
</script>

<style lang="scss">
  @import './styles/main.scss';
  @import './styles/monokai.scss';
</style>
