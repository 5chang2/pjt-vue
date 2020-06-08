<template>
  <div id="app">
    <div id="nav">
      <router-link to="/">Home</router-link> |
      <span v-if="isLogin">
        <router-link to="/articles">Articles</router-link> |
        <router-link to="/articles/create">Create</router-link> |
        <!-- <router-link to="/accounts/logout" @click="logout">logout</router-link> -->
        <span @click="logout">logout</span>
      </span>
      <span v-else>
        <router-link to="/accounts/signup">signup</router-link> |
        <router-link to="/accounts/login">login</router-link>
      </span>
    </div>
    <router-view @onLogin="login" @onSignup="signup"/>
  </div>
</template>

<script>
import axios from 'axios'

const BACK_URL = 'http://127.0.0.1:8000'

export default {
  name: "App",
  data() {
    return {
      isLogin: false
    }
  },
  mounted() {
    if(this.$cookies.isKey('auth-token')) {
      this.isLogin = true
    }
    else {
      this.isLogin = false
    }
  },
  methods:{
    signup(info) {
      const signupForm = new FormData()
      signupForm.append('username', info.username)
      signupForm.append('password1', info.password1)
      signupForm.append('password2', info.password2)

      axios.post(`${BACK_URL}/rest-auth/signup/`, signupForm)
        .then((res)=>{
          this.$cookies.set('auth-token', res.data.key)
          console.log(res.data)
          this.isLogin = true
          this.$router.push('/')
        })
        .catch((err)=>{
          console.log(err.response.data)
        })
    },
    login(info) {
      const loginForm = new FormData()
      loginForm.append('username', info.username)
      loginForm.append('password', info.password)

      axios.post(`${BACK_URL}/rest-auth/login/`, loginForm)
        .then((res)=>{
          console.log(res.data.key)
          this.$cookies.set('auth-token', res.data.key)
          this.isLogin = true
          this.$router.push('/')

          // this.$cookies.set('auth-token', res.data.key, 5)
          // this.$cookies.set("default_unit_second","input_value");            // 1 second after, expire

        })
        .catch((err)=>{
          console.log(err.response)
        })
    },
    logout(){
      const requestHeaders = {
        headers: {
          Authorization: `token ${this.$cookies.get('auth-token')}`
        }
      }

      axios.post(`${BACK_URL}/rest-auth/logout/`, requestHeaders)
        .then((res)=>{
          this.$cookies.remove('auth-token')
          console.log(res.response)
          this.isLogin = false
          this.$router.push('/')

        })
        .catch((err)=>{
          console.log(err.response)
        })
    }
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
}

#nav {
  padding: 30px;
}

#nav a {
  font-weight: bold;
  color: #2c3e50;
}

#nav a.router-link-exact-active {
  color: #42b983;
}
</style>
