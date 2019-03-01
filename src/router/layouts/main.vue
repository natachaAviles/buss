<script>
import NavBar from '@components/nav-bar'
import Footer from '@components/footer-main'

export default {
  components: { NavBar, Footer },
}
</script>

<template>
  <div>
    <NavBar />
    <slot />
    <Footer />
  </div>
</template>

<style lang="scss" module>
@import '@design';
</style>
