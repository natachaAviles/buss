<script>
import appConfig from '@src/app.config'
import Layout from '@layouts/main'

export default {
  page: {
    title: 'Home',
    meta: [{ name: 'description', content: appConfig.description }],
  },
  components: { Layout },
}
</script>

<template>
  <Layout>
    <section :class="$style.hero_main_nav" class=" is-fullheight-with-navbar">
      <div class="hero-body">
        <div class="container"></div>
      </div>
    </section>
  </Layout>
</template>

<style>
.hero_main_nav {
  background-image: linear-gradient(
      to right,
      rgba(95, 186, 125, 0.9) 0%,
      rgba(16, 138, 236, 0.9) 100%
    ),
    url('https://freedesignfile.com/upload/2015/07/Modern-Isometric-buildings-model-vector-material-06.jpg');
  background-position: center;
}
</style>
