<template>
  <div>
    <h1>login</h1>
    username : <input type="text" v-model="username">
    <br>
    password : <input type="password" v-model="password">
    <br>
    <button @click="login">login</button>
  </div>
</template>

<script>
// import axios from 'axios'

// const BACK_URL = 'http://127.0.0.1:8000'

export default {
  name: 'SignupView',
  data() {
    return {
      username: null,
      password: null,
    }
  },
  methods: {
    login() {
      this.$emit('onLogin', {username: this.username, password: this.password})
    }
    // login() {
    //   const loginForm = new FormData()
    //   loginForm.append('username', this.username)
    //   loginForm.append('password', this.password)

    //   axios.post(`${BACK_URL}/rest-auth/login/`, loginForm)
    //     .then((res)=>{
    //       console.log(res.data.key)
    //       this.$cookies.set('auth-token', res.data.key)
    //       // this.$cookies.set('auth-token', res.data.key, 5)
    //       // this.$cookies.set("default_unit_second","input_value");            // 1 second after, expire

    //     })
    //     .catch((err)=>{
    //       console.log(err.response)
    //     })
    // }
  },
  // computed: {
  //   jsonSignupInfo(){
  //     return JSON.stringify(this.signupInfo)
  //   }
  // }
}
</script>

<style>

</style>