<template lang="pug">
    v-layout(column)
        v-flex(xs6 offset-xs3)
            panel(title='Register')
                form(name='register', autocomplete='off' slot="main")
                    v-text-field.input-group(type="email", name="email", label="email", v-model="email")
                    br
                    v-text-field.input-group(type="password", name="password", label="password", v-model="password",autocomplete="new-password")
                    br
                    .error(v-html='error')
                    v-btn.cyan(dark, @click="register") Register
</template>

<script>
  import AuthenticationService from '@/services/AuthenticationService'
  import Panel from '@/components/Panel'
  export default {
    name: 'Register',
    data () {
      return {
        email: '',
        password: '',
        error: null
      }
    },
    components: {
      Panel
    },
    watch: {
      email: (value) => {
        console.log(`email has changed`, value)
      }
    },
    methods: {
      async register () {
        try {
          const response = await AuthenticationService.register({
            email: this.email,
            password: this.password
          })
          this.$store.dispatch('setToken', response.data.token)
          this.$store.dispatch('setUser', response.data.user)
        } catch (error) {
          this.error = error.response.data.error
        }
      }
    },
    mounted () {

    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss" scoped>

</style>
