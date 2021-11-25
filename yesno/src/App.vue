<template>
  <div id="app">
    <div>Do you love croissant?</div>
    <div v-show="!answered">Well{{ dots }}</div>
    <div>{{ answer }}</div>
    <div>
      <button @click="setAnswer('Yes I love croissant')">Yes</button>
      <button @click="setAnswer('No I don\'t like croissant :(')">No</button>
      <button @click="reset()">Again</button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'App',
  mounted() {
    setInterval(() => {
      if (this.dots.length > 2) {
        this.dots = '.'
      } else {
        this.dots += '.'
      }
    }, 1000)
  },
  data() {
    return {
      answered: false,
      answer: '',
      dots: '.',
    }
  },
  methods: {
    setAnswer(text) {
      if (this.answered) {
        return
      }
      this.answer = text
      this.answered = true
    },
    reset() {
      this.answer = ''
      this.answered = false
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  font-size: 3rem;
}
</style>
