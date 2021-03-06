<template>
<section :id="!userId ? 'not-logged-in' : undefined">
  <logged-in 
    v-if="userId"
    bottomText="If you want to create another account, please logout first."
  />
  <div v-else>
    <img
      src="~/assets/images/logo-light.svg"
      alt="corum"
    >
    <form>
      <input
        v-model.trim="email"
        type="email"
        placeholder="Email Address"
        onfocus="this.placeholder=''" 
        onblur="this.placeholder='Email Address'"
        autocapitalize="off"
        spellcheck="false"
      >
      <input
        v-model.trim="username"
        type="text"
        placeholder="Username"
        onfocus="this.placeholder=''" 
        onblur="this.placeholder='Username'"
      >
      <input
        v-model.trim="password1"
        type="password"
        placeholder="Password"
        onfocus="this.placeholder=''" 
        onblur="this.placeholder='Password'"
      >
      <input
        v-model.trim="password2"
        type="password"
        placeholder="Confirm Password"
        onfocus="this.placeholder=''" 
        onblur="this.placeholder='Confirm Password'"
      >
      <input
        v-if="loading"
        type="submit" 
        value="Please Wait..." 
        @click.prevent
        class="disabled-button"
        title="Waiting for a response from the server..."
      >
      <input
        v-else-if="correctDetails"
        type="submit"
        value="Sign Up"
        @click.prevent="signup"
        class="enabled-button"
      >
      <input
        v-else
        type="submit"
        value="Sign Up"
        @click.prevent
        class="disabled-button"
        title="You have entered something wrong, please try again"
      >
    </form>
    <p>Already have an account?<br><nuxt-link to="/login">Login</nuxt-link></p>
  </div>
</section>
</template>

<script>
import LoggedIn from '~/components/LoggedIn'
import signupUserMutation from '~/apollo/mutations/signupUser'

import alertError from '~/utils/alertError'

export default {
  components: {
    LoggedIn
  },

  head: () => ({ title: 'Sign Up' }),

  computed: {
    correctDetails() {
      const emailEntered = this.email !== ''
      const usernameEntered = this.username !== ''

      const password1Entered = this.password1 !== ''
      const password2Entered = this.password2 !== ''
      const passwordsEntered = password1Entered && password2Entered
      const passwordsMatch = this.password1 === this.password2

      return (
        emailEntered && usernameEntered && passwordsEntered && passwordsMatch
      )
    },

    password() {
      const { password1, password2 } = this
      if (password1 === password2) {
        return password1
      }
    },

    // Gets the user's ID from the vuex store
    userId() {
      return this.$store.state.userId
    }
  },

  data() {
    return {
      email: '',
      username: '',
      password1: '',
      password2: '',
      loading: false
    }
  },

  methods: {
    async signup() {
      try {
        // Renders the loading submit button
        this.loading = true

        const { username, email, password } = this

        const { data: { signupUser } } = await this.$apollo.mutate({
          mutation: signupUserMutation,
          variables: {
            username,
            email,
            password
          }
        })

        const { id, token } = signupUser
        this.$store.commit('saveUserData', { id, username, token })

        // Returns the user to the page they were on before
        // TODO: If user was on signup before, redirect to home
        this.$router.back()
      } catch ({ message }) {
        // Renders the normal submit button
        this.loading = false

        // Display the error to the user in an alert box
        alertError(message)
      }
    }
  }
}
</script>

<style lang="stylus" scoped>
@require '../assets/styles/accountPages'
</style>
