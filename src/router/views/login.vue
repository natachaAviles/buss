<script>
import Layout from '@layouts/main'
import { authMethods } from '@state/helpers'
import appConfig from '@src/app.config'

export default {
  page: {
    title: 'Log in',
    meta: [{ name: 'description', content: `Log in to ${appConfig.title}` }],
  },
  components: { Layout },
  data() {
    return {
      username: '',
      password: '',
      authError: null,
      tryingToLogIn: false,
    }
  },
  computed: {
    placeholders() {
      return process.env.NODE_ENV === 'production'
        ? {}
        : {
            username: 'Use "admin" to log in with the mock API',
            password: 'Use "password" to log in with the mock API',
          }
    },
  },
  methods: {
    ...authMethods,
    // Try to log the user in with the username
    // and password they provided.
    tryToLogIn() {
      this.tryingToLogIn = true
      // Reset the authError if it existed.
      this.authError = null
      return this.logIn({
        username: this.username,
        password: this.password,
      })
        .then((token) => {
          this.tryingToLogIn = false

          // Redirect to the originally requested page, or to the home page
          this.$router.push(
            this.$route.query.redirectFrom || { name: 'profile' }
          )
        })
        .catch((error) => {
          this.tryingToLogIn = false
          this.authError = error
        })
    },
  },
}
</script>

<template>
  <Layout :class="$style.form">
    <div class="container">
      <div class="card animated fadeInDownBig" :class="$style.logincard">
        <div class="card-image"> </div>
        <div class="card-content">
          <form @submit.prevent="tryToLogIn">
            <BaseInputText
              v-model="username"
              name="username"
              :placeholder="placeholders.username"
            />
            <BaseInputText
              v-model="password"
              name="password"
              type="password"
              :placeholder="placeholders.password"
            />
            <BaseButton
              :disabled="tryingToLogIn"
              type="submit"
              class="button is-primary is-rounded"
            >
              <BaseIcon v-if="tryingToLogIn" name="sync" spin />
              <span v-else>
                INGRESAR
              </span>
            </BaseButton>
            <p v-if="authError" :class="$style.autherror">
              There was an error logging in to your account.
            </p>
          </form>
        </div>
      </div>
    </div>
  </Layout>
</template>

<style lang="scss" module>
@import '@design';

.form {
  background-image: linear-gradient(
      to right,
      rgba(95, 186, 125, 0.9) 0%,
      rgba(16, 138, 236, 0.9) 100%
    ),
    url('https://freedesignfile.com/upload/2015/07/Modern-Isometric-buildings-model-vector-material-06.jpg');
  background-position: center;
}
.logincard {
  max-width: 600px;
  margin: 100px auto 200px auto;
  border: none;
  border-radius: 5px;
  box-shadow: 0 5px 20px 0 rgba(0, 0, 0, 0.1);
}
.logincard input {
  border: 1px solid rgba(95, 186, 125, 0.9);
}
.logincard input:focus {
  border: 1px solid rgba(95, 186, 125, 0.9);
  box-shadow: 0 5px 20px 0 rgba(0, 0, 0, 0.1);
}
.autherror {
  font-size: 12px;
  color: hsl(348, 100%, 61%);
}
</style>
