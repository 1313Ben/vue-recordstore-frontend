<template>
<div class="max-w-sm m-auto my-8">
  <div class="border p-10 border-grey-light shadow rounded">
    <h3 class="text-2xl mb-6 text-grey-darkest">Sign In</h3>
    <form @submit.prevent="signin">
      <div class="text-red" v-if="error">
          {{ error }}
      </div>

      <div class="mb-6">
        <label for="email">E-mail</label>
        <input type="email" v-model="email" class="input" id="email" placeholder="a@a.com">
      </div>

      <div class="mb-6">
        <label for="password">Password</label>
        <input type="password" v-model="password" class="input" id="password" placeholder="password">
      </div>

      <button type="submit" >
        Sign In
      </button>

      <div class="my-4">
        <router-link to="/signup" class="link">Sign Up</router-link>
      </div>

    </form>
  </div>
</div>
</template>

<script>
export default {
  name: 'Signin',
  data () {
    return {
      email: '',
      password: '',
      error: ''
    }
  },
  created () {
    this.checkSignedIn()
  },
  updated () {
    this.checkSignedIn()
  },
  methods: {
    signin () {
      this.$http.plain.post('/signin', {email: this.email, password: this.password})
        .then(response => this.signinSuccessful(response))
        .catch(error => this.signinFailed(error))
    },
    signinSuccessful (response) {
      if (!response.data.csrf) {
        this.signinFailed(response)
        return
      }
      localStorage.csrf = response.data.csrf
      localStorage.signedIn = true
      this.error = ''
      this.$router.replace('/records')
    },
    signinFailed (error) {
      this.error = (error.response && error.response.data && error.response.data.error) || ''
      delete localStorage.csrf
      delete localStorage.signedIn
    },
    checkSignedIn () {
      if (localStorage.signedIn) {
        this.$router.replace('/records')
      }
    }
  }
}
</script>
