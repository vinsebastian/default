<template>
  <section class="content">
    <div class="container page-content">
      <h2>Password assistance</h2>
      <form class="w-full" @submit="authenticate">
        <div class="md:flex md:items-center mb-2">
          <div class="md:w-1/3"></div>
          <div class="md:w-2/3">
            <p>Enter the email address associated with your account. We will emailing you the password reset link.</p>
            <p v-if="error" class="text-red font-bold">{{ error }}</p>
          </div>
        </div>
        <div class="md:flex md:items-center mb-6">
          <div class="md:w-1/3">
            <label
              class="block text-grey font-bold md:text-right mb-1 md:mb-0 pr-4"
              for="inline-email"
            >Email</label>
          </div>
          <div class="md:w-2/3">
            <input
              id="inline-email"
              v-model="email"
              :disabled="process"
              class="bg-grey-lighter appearance-none border-2 border-grey-lighter rounded w-full py-2 px-4 text-grey-darker leading-tight focus:outline-none focus:bg-white focus:border-teal"
              type="email"
              required
            >
          </div>
        </div>
        <div class="md:flex md:items-center">
          <div class="md:w-1/3"></div>
          <div class="md:w-1/3">
            <button
              class="w-full md:w-auto shadow bg-teal hover:bg-teal-light focus:shadow-outline focus:outline-none text-white font-bold py-2 px-4 rounded"
              type="submit"
            >Send Password Reset Link</button>
          </div>
          <div class="md:w-1/3">
            <nuxt-link class="w-full mt-3 md:m-0 md:w-auto text-center inline-block align-baseline font-bold text-sm text-grey-darker hover:text-teal-dark no-underline md:float-right" to="/signin">
              Signin
            </nuxt-link>
          </div>
        </div>
      </form>
    </div>
  </section>
</template>
<script>
export default {
  head() {
    return {
      title: 'Reset Password'
    }
  },
  data: () => ({
    process: false,
    error: null,
    email: ''
  }),
  methods: {
    async authenticate(e) {
      e.preventDefault()
      try {
        this.process = true
        await this.$axios.post('/api/send-password-reset-link', {
          email: this.email
        })
      } catch (e) {
        this.process = false
        this.error =
          'Invalid credentials, we can not found user with that email'
      }
    }
  }
}
</script>
