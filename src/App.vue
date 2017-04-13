<template>
  <div id="app">
    <target :size="600"></target>
  </div>
</template>

<script>
import Target from './components/Target'

export default {
  name: 'app',
  components: {
    Target
  }
}
</script>
