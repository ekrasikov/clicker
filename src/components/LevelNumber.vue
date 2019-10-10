<template>
  <div class="has-text-centered">
    <p class="title">{{ Level }}</p>
    <p class="subtitle">level</p>
  </div>
</template>

<script>
import { EventBus } from '../utils/event-bus.js';

export default {
  components: {
  },
  methods: {
    incrementLevel(payload) {
      this.Level++;
      // eslint-disable-next-line
      console.log(payload)
      // submit event level-up with new level number
      EventBus.$emit('level-up', { levelNum: this.Level });
    }
  },
  mounted () {
    // go to the next level on level-done event
    EventBus.$on('level-done', (payload) => { this.incrementLevel(payload) })
  },
  data () {
    return {
      Level: 1
    }
  }
}
</script>

<style>
</style>