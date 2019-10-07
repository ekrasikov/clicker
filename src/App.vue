<template>
  <section id="app" class="section">
    <div class="container is-fluid">
      <div class="tile is-ancestor">
        <div class="tile is-vertical is-parent">
          <div class="tile is-parent">
            <div class="tile is-child box">
              <Message/>
            </div>
          </div>
          <div class="tile is-parent">
            <div class="tile is-child box">
              <LevelNumber/>
            </div>
            <div class="tile is-child is-6 box">
              <LevelProgress/>
            </div>
            <div class="tile is-child box">
              <!---->
            </div>
          </div>
          <div class="tile is-parent">
            <div class="tile is-child box">
              <div class="buttons">
                <b-button type="is-primary" @click="manualClick()">Click me!</b-button>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script>
import LevelNumber from './components/LevelNumber.vue'
import LevelProgress from './components/LevelProgress.vue'
import Message from './components/Message.vue'
import { EventBus } from './utils/event-bus.js';

export default {
  name: 'app',
  components: {
    LevelNumber,
    LevelProgress,
    Message
  },
  data () {
    return {
      manualClickAmount: 2
    }
  },
  methods: {
    manualClick() {
      EventBus.$emit('manual-click', this.manualClickAmount)
    }
  }
}
</script>

<style lang="scss">
@import "~bulma/sass/utilities/_all";
$gap: 128px;
@import "~bulma";
@import "~buefy/src/scss/buefy";
</style>
