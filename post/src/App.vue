<template>
  <div id="app">
    <button @click="loadData()">click me</button>
    <div v-if="loading">loading...</div>
    <div v-else>
      <div class="post" v-for="item in data" :key="item.id">
        <h2>{{ item.title }}</h2>
        <p>{{ item.body }}</p>
      </div>
    </div>
  </div>
</template>

<script>

export default {
  name: 'App',
  data() {
    return {
      data: [],
      loading: false
    }
  },
  methods: {
    loadData() {
      this.loading = true
      fetch('https://jsonplaceholder.typicode.com/posts')
        .then((response) => {
          return response.json()
        })
        .then((data) => {
          this.data = data
        })
        .catch((err) => {
          alert(err)
        })
        .finally(() => {
          this.loading = false
        })
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 16px;
}
.post {
  border: 2px solid #999;
  border-radius: 4px;
  padding: 8px;
  margin-top: 16px;
}
</style>
