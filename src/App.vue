<template>
<div id="app">
  <pulpul
    :topMethod=refresh
    :bottomMethod="loadMore"
    :distance2Fire=50>
    <ul>
      <li v-for="(item, i) in items">
        # item {{ i }} 
      </li>
    </ul>
  </pulpul>
</div>
</template>

<script>
import Pulpul from './Pulpul.vue'

export default {
  components: { Pulpul },

  data () {
    return {
      items: Array.from(Array(10)),
    }
  },

  methods: {
    refresh() {
      this.items = Array.from(Array(10))
    },

    loadMore() {
      this.items = [
        ...this.items,
        ...[10, 11]
      ]
    }
  },
}
</script>

<style>
body {
  font-family: Helvetica, sans-serif;
}

#app {
  height: 400px;
  overflow: scroll;
}
</style>
