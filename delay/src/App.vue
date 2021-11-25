<template>
  <div id="app">
    <div>Do you love croissant?</div>
    <div v-show="show">Well...</div>
    <div v-show="!show">Yes I love croissant</div>
  </div>
</template>

<script>

export default {
  name: 'App',
  data() {
    return {
      show: true
    }
  },
  beforeCreate() {
    console.log('beforeCreate', this.show)
  },
  created() {
    console.log('created', this.show)
  },
  beforeMount() {
    console.log('beforeMount', this.show, this.$el)
  },
  mounted() {
    setTimeout(() => {
        this.show = false
    }, 3000)
  },
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
  font-size: 40px;
}
</style>
