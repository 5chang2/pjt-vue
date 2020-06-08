<template>
  <div>
    <h1>signup</h1>
    username : <input type="text" v-model="signupInfo.username">
    <br>
    password : <input type="password" v-model="signupInfo.password1">
    <br>
    password2 : <input type="password" v-model="signupInfo.password2">
    <br>
    <button @click="signup">signup</button>
  </div>
</template>

<script>
// import axios from 'axios'

// const BACK_URL = 'http://127.0.0.1:8000'

export default {
  name: 'SignupView',
  data() {
    return {
      signupInfo: {
        username: null,
        password1: null,
        password2: null
      }
    }
  },
  methods: {
    signup() {
      this.$emit('onSignup', this.signupInfo)
    }
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