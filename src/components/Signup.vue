<template>
<div class="max-w-sm m-auto my-8">
  <div class="border p-10 border-grey-light shadow rounded">
    <h3 class="text-2xl mb-6 text-grey-darkest">Sign Up</h3>
    <form @submit.prevent="signup">
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

      <div class="mb-6">
        <label for="password">Password Confirmation</label>
        <input type="password" v-model="password_confirmation" class="input" id="password_confirmation" placeholder="password_confirmation">
      </div>

      <button type="submit" >
        Sign Up
      </button>

      <div class="my-4">
        <router-link to="/" class="link">Sign In</router-link>
      </div>

    </form>
  </div>
</div>
</template>

<script>
export default {
  name: 'Signup',
  data () {
    return {
      email: '',
      password: '',
      password_confirmation: '',
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
      this.$http.plain.post('/signup', {email: this.email, password: this.password, password_confirmation: this.password_confirmation})
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
      this.error = (error.response && error.response.data && error.response.data.error) || 'Something went wrong'
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
