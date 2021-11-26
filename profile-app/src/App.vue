<template>
  <div id="app">
    <form @submit.prevent="register">
      <div>
        <label for="firstname">First Name: </label>
        <input type="text" id="firstname" v-model="form.firstname">
      </div>
      <div>
        <label for="lastname">Last Name: </label>
        <input type="text" id="lastname" v-model="form.lastname">
      </div>
      <div>
        <label for="phoneNumber">Phone Number: </label>
        <input type="text" id="phoneNumber" v-model="form.phoneNumber">
      </div>
      <div>
        <label for="email">Email: </label>
        <input type="email" id="email" v-model="form.email">
      </div>
      <div>
        <label for="password">Password: </label>
        <input type="password" id="password" v-model="form.password">
      </div>
      <div>
        <label for="avatar">Avatar: </label>
        <input type="file" id="avatar" @change="addAvatar">
      </div>
      <div>
        <button>submit</button>
      </div>
    </form>
    <br>
    <hr>
    <br>
    <form @submit.prevent="login">
      <div>
        <label for="username">Username: </label>
        <input type="text" id="username" v-model="loginForm.username">
      </div>
      <div> 
        <label for="password">Password: </label>
        <input type="password" id="password" v-model="loginForm.password">
      </div>
      <div>
        <button>login</button>
      </div>
      <div>Token: {{ token || 'not login yet' }}</div>
    </form>
    <div v-if="token">
      <div>First Name: {{ profile.firstname }}</div>
      <div>Last Name: {{ profile.lastname }}</div>
      <div>Phone Name: {{ profile.phoneNumber }}</div>
      <div>Email: {{ profile.email }}</div>
      <div v-if="profile.avatar">Avatar: <img :src="profileAvatar" width="100"></div>
    </div>
  </div>
</template>

<script>

// const host = 'https://camt-foodapi.pair-co.com'
const host = 'http://localhost:3000'

export default {
  name: 'App',
  data() {
    return {
      form: {
        firstname: 'Peerawat',
        lastname: 'Poombua',
        phoneNumber: '1324131515',
        email: 'pongneng+3@test.com',
        password: 'abcd1234',
        avatar: '',
      },
      token: '',
      profile: {},
      loginForm: {
        username: 'pongneng+3@test.com',
        password: 'abcd1234',
      }
    }
  },
  methods: {
    register() {
      const formData = new FormData()
      formData.append('firstname', this.form.firstname)
      formData.append('lastname', this.form.lastname)
      formData.append('email', this.form.email)
      formData.append('phoneNumber', this.form.phoneNumber)
      formData.append('password', this.form.password)
      formData.append('avatar', this.form.avatar)

      const registerURL = `${host}/register`
      fetch(registerURL, {
        method: 'POST',
        body: formData
      }).then(response => {
        if (response.status >= 200 && response.status < 300) {
          alert('ok')
        } else {
          alert('fail')
        }
      }).catch(err => {
        alert(err)
      })
    },
    addAvatar(event) {
      this.avatar = event.target.files[0]
      console.log(this.avatar)
    },
    login() {
      const loginURL = `${host}/login`
      fetch(loginURL, {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json'
        },
        body: JSON.stringify({ ...this.loginForm })
      }).then(response => {
        if (response.status >= 200 && response.status < 300) {
          return response.json()
        } else {
          return Promise.reject('status is not 200 range')
        }
      }).then(json => {
        this.token = json.token
        return this.loadProfile()
      })
      .catch(err => {
        alert(err)
      })
    },
    loadProfile() {
      const profileURL = `${host}/me`
      fetch(profileURL, {
        headers: {
          'Authorization': `Bearer ${this.token}`
        }
      }).then(response => {
        if (response.status >= 200 && response.status < 300) {
          return response.json()
        }
        return Promise.reject('status is not 200 range')
      })
      .then(profile => {
        this.profile = profile
      })
      .catch(err => {
        alert(err)
      })
    },
    profileAvatar() {
      return `${host}${this.profile.avatar}`
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
}
</style>
