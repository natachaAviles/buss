<script>
import { authComputed } from '@state/helpers'
import NavBarRoutes from './nav-bar-routes'

export default {
  components: { NavBarRoutes },
  data() {
    return {
      // persistentNavRoutes: [
      // {
      // name: 'home',
      // title: 'Home',
      // },
      // ],
      loggedInNavRoutes: [
        {
          name: 'profile',
          title: () => 'Sesión iniciada como ' + this.currentUser.name,
        },
        {
          name: 'logout',
          title: 'Log out',
        },
      ],
      // loggedOutNavRoutes: [
      // {
      // name: 'login',
      // title: 'Log in',
      // },
      // ],
    }
  },
  computed: {
    ...authComputed,
  },
  mounted() {
    this.navbarInit()
  },
  methods: {
    navbarInit() {
      document.addEventListener('DOMContentLoaded', () => {
        // Get all "navbar-burger" elements
        const $navbarBurgers = Array.prototype.slice.call(
          document.querySelectorAll('.navbar-burger'),
          0
        )

        // Check if there are any navbar burgers
        if ($navbarBurgers.length > 0) {
          // Add a click event on each of them
          $navbarBurgers.forEach((el) => {
            el.addEventListener('click', () => {
              // Get the target from the "data-target" attribute
              const target = el.dataset.target
              const $target = document.getElementById(target)

              // Toggle the "is-active" class on both the "navbar-burger" and the "navbar-menu"
              el.classList.toggle('is-active')
              $target.classList.toggle('is-active')
            })
          })
        }
      })
    },
  },
}
</script>

<template>
  <nav class="navbar" :class="$style.main_navbar">
    <div class="navbar-brand">
      <a class="navbar-item">
        <p :class="$style.navbar_title">ReadyBUSS</p>
      </a>
      <div class="navbar-burger burger" data-target="navbarExampleTransparentExample">
        <span></span>
        <span></span>
        <span></span>
      </div>
    </div>

    <div id="navbarExampleTransparentExample" class="navbar-menu">
      <div class="navbar-end">
        <div class="navbar-item">
          <div class="field is-grouped">
            <p class="control">
              <!--<NavBarRoutes :routes="persistentNavRoutes" />-->
            </p>
            <p class="control">
              <router-link
                v-if="loggedIn"
                class="navbar-item button is-primary is-rounded is-outlined"
                to="logout"
              >Cerrar sesión</router-link>
            </p>
          </div>
        </div>
      </div>
    </div>
  </nav>
</template>

<style lang="scss" module>
@import '@design';
.main_navbar {
  background-color: #223344;
}
.navbar_title {
  font-family: 'Grand Hotel', cursive;
  font-size: 30px;
  color: #ffffff;
}
</style>
