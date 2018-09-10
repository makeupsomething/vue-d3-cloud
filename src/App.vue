<template>
  <div id="app">
    <cloud :data="words" :fontSizeMapper="fontSizeMapper" :rotate="rotate" />
  </div>
</template>

<script>
import Cloud from './components/Cloud.vue'

export default {
  name: 'app',
  data() {
    return {
      words: [
        { text: 'Hey', value: 1000 },
        { text: 'lol', value: 200 },
        { text: 'first impression', value: 800 },
        { text: 'very cool', value: 1000000 },
        { text: 'duck', value: 10 },
      ],
      fontSizeMapper: word => Math.log2(word.value) * 5,
      rotate: 0,
    }
  },

  components: {
    Cloud
  }
}
</script>
