<template>
  <div>
    <p class="subtitle has-text-centered">Level progress: {{ currentValue }}/{{ previousValue }}</p>
    <progress class="progress is-warning" :value=currentValue v-bind:max=previousValue></progress>
  </div>
</template>

<script>
import { EventBus } from '../utils/event-bus.js';

export default {
  components: {
  },
  data () {
    return {
      previousValue: 10,
      currentValue: 7,
      newLevelCoefficient: 1.5
    }
  },
  mounted () {
    EventBus.$on('manual-click', this.decreaseValue)
  },
  methods: {
    decreaseValue (amount) {
      // if there's enough amount in a level, decrease
      if (this.currentValue >= amount)
        this.currentValue = this.currentValue - amount;
      // otherwise start new level
      else {
        // emit event to start new level
        EventBus.$emit('level-done', {});
        this.currentValue = Math.floor(this.previousValue * this.newLevelCoefficient);
        this.previousValue = this.currentValue;
      }
    }
  }
}
</script>

<style>
</style>