<template>
  <div id="app">
    <div id="nav">
      <router-link to="/">Home</router-link> |
      <span v-if="isLogin">
        <router-link to="/articles">Articles</router-link> |
        <router-link to="/articles/create">Create</router-link> |
        <router-link to="/articles/logout" @click.native="logout">logout</router-link>
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
    // key를 받아서 cookie에 저장 후 로그인 상태 
    setCookie(key){
      // default expire time: 1 day
      // this.$cookies.set('auth-token', key, 60 * 60 * 24) => 60초 * 60분 * 24시간 => 하루뒤 만료
      this.$cookies.set('auth-token', key)
      this.isLogin = true
    },
    // auth-token 쿠키를 삭제 후 로그아웃 상태
    removeCookie(){
      this.$cookies.remove('auth-token')
      this.isLogin = false
    },
    signup(info) {
      const signupForm = new FormData()
      signupForm.append('username', info.username)
      signupForm.append('password1', info.password1)
      signupForm.append('password2', info.password2)

      axios.post(`${BACK_URL}/rest-auth/signup/`, signupForm)
        .then((res)=>{
          this.setCookie(res.data.key)
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
          this.setCookie(res.data.key)
          this.$router.push('/')
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
        .then(()=>{
          this.removeCookie()
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
