
<template>
  <main-screen v-if="statusMatch === 'default'" @onStart="onHandleBeforStart($event)" />
  <interact-screen
    v-if="statusMatch === 'match'"
    :cardsContext="settings.cardsContext"
    @onFinish="onGetResult"
  />
  <result-screen v-if="statusMatch === 'result'" @onStartAgain="statusMatch = 'default'" />
  <coppy-right />
</template>

<script>
import MainScreen from './components/MainScreen.vue'
import InteractScreen from './components/InteractScreen.vue'
import ResultScreen from './components/ResultScreen.vue'
import CoppyRight from './components/CoppyRight.vue'

import { shuffled } from './utils/array'

export default {
  name: 'App',
  components: {
    MainScreen,
    InteractScreen,
    ResultScreen,
    CoppyRight,
  },
  data() {
    return {
      settings: {
        totalOfBlocks: 0,
        cardsContext: [],
        startedAt: null,
      },
      statusMatch: 'default',
      timer: 0,
    }
  },
  methods: {
    onHandleBeforStart(config) {
      console.log('running handle before start', config)
      this.settings.totalOfBlocks = config.totalOfBlocks
      const firstCards = Array.from({ length: this.settings.totalOfBlocks / 2 }, (_, i) => i + 1)
      const secondCards = [...firstCards]
      const cards = [...firstCards, ...secondCards]
      this.settings.cardsContext = shuffled(shuffled(shuffled(shuffled(cards))))
      this.settings.startedAt = new Date().getTime()
      this.statusMatch = 'match'
    },
    onGetResult() {
      this.time = new Date().getTime() - this.settings.startedAt
      this.statusMatch = 'result'
    },
  },
}
</script>