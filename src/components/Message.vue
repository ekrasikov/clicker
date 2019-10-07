<template>
  <div>
    <p>{{ MessageText }}</p>
  </div>
</template>

<script>

import { EventBus } from '../utils/event-bus.js';

const messages = [
  "Come on, it's useless!",
  "Good job!",
  "Click harder",
  "That's the spirit!",
  "One more click",
  "Yeah, baby!",
  "Move on, don't stop",
  "Click it",
  "Schneller!",
  "Faster!",
  "Come on, you can do it!",
  "Go get them!",
  "Yes, you can!",
  "Click and tap",
  "Try harder!"
]

export default {
  components: {
  },
  data () {
    return {
      MessageText: "Hello!"
    }
  },
  mounted () {
    EventBus.$on('manual-click', this.updateText)
  }, 
  methods: {
    updateText () {
      this.MessageText = messages[Math.floor(Math.random() * messages.length)]
    }
  }
}
</script>

<style>
</style>