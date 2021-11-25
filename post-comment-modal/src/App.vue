<template>
  <div id="app">
    <div class="post" v-for="post in posts" :key="post.id">
      <div class="title">{{ post.title }}</div>
      <div class="body">{{ post.body }}</div>
      <div><button @click="showComment(post.id)">view comment</button></div>
    </div>
    <div id="myModal" class="modal" :style="{ 'display': isShowComment ? 'block' : 'none' }">
      <!-- Modal content -->
      <div class="modal-content">
        <span class="close" @click="hideComment()">&times;</span>
        <div class="container">
          <div class="post" v-for="comment in comments" :key="comment.id">
            <div class="name">{{ comment.name }}</div>
            <div class="email">{{ comment.email }}</div>
            <div class="body">{{ comment.body }}</div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>

export default {
  name: 'App',
  components: {
  },
  data() {
    return {
      posts: [],
      comments: [],
      isShowComment: false
    }
  },
  methods: {
    showComment(postId) {
      console.log(`https://jsonplaceholder.typicode.com/posts/${postId}/comments`)
      fetch(`https://jsonplaceholder.typicode.com/posts/${postId}/comments`)
        .then(response => response.json())
        .then(commentData => {
          this.comments = commentData
          this.isShowComment = true
        })
    },
    hideComment() {
      this.isShowComment = false
    }
  },
  mounted() {
    fetch('https://jsonplaceholder.typicode.com/posts')
      .then(response => response.json())
      .then(postData => {
        this.posts = postData
      })
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}
.post {
  padding: 16px;
  border: 2px solid black;
  border-radius: 4px;
  margin-top: 16px;
}
.title, .name {
  font-weight: 700;
  font-size: 20px;
}
.email {
  font-style: italic;
  color: #888;
}
.container {
  margin-top: 40px;
}
/* The Modal (background) */
.modal {
  display: none; /* Hidden by default */
  position: fixed; /* Stay in place */
  z-index: 1; /* Sit on top */
  left: 0;
  top: 0;
  width: 100%; /* Full width */
  height: 100%; /* Full height */
  overflow: auto; /* Enable scroll if needed */
  background-color: rgb(0,0,0); /* Fallback color */
  background-color: rgba(0,0,0,0.4); /* Black w/ opacity */
}

/* Modal Content/Box */
.modal-content {
  background-color: #fefefe;
  margin: 15% auto; /* 15% from the top and centered */
  padding: 20px;
  border: 1px solid #888;
  width: 80%; /* Could be more or less, depending on screen size */
}

/* The Close Button */
.close {
  color: #aaa;
  float: right;
  font-size: 28px;
  font-weight: bold;
}

.close:hover,
.close:focus {
  color: black;
  text-decoration: none;
  cursor: pointer;
}
</style>
