<template>
  <div>
    <p class="title">{{ Level }}</p>
    <p class="subtitle">level</p>
  </div>
</template>

<script>
import { EventBus } from '../utils/event-bus.js';

export default {
  components: {
  },
  mounted () {
    EventBus.$on('level-up', this.upLevel)
  },
  data () {
    return {
      Level: 1
    }
  },
  methods: {
    upLevel () {
      this.Level++;
    }
  }
}
</script>

<style>
</style>