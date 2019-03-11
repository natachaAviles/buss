<script>
import Layout from '@layouts/main'
import axios from 'axios'
import choferAdmin from '@components/choferAdmin'
import busAdmin from '@components/busAdmin'
import horarioAdmin from '@components/horarioAdmin'
import trayectoAdmin from '@components/trayectoAdmin'
import navbarLeft from '@components/navbar-left'

export default {
  page() {
    return {
      title: this.user.name,
      meta: [
        {
          name: 'description',
          content: `The user profile for ${this.user.name}.`,
        },
      ],
    }
  },
  components: {
    Layout,
    busAdmin,
    horarioAdmin,
    trayectoAdmin,
    choferAdmin,
    navbarLeft,
  },
  props: {
    user: {
      type: Object,
      required: true,
    },
  },
}
</script>

<template>
  <Layout>
    <choferAdmin/>
    <busAdmin/>
    <horarioAdmin/>
    <trayectoAdmin/>
  </Layout>
</template>

<style lang="scss" module>
@import '@design';
</style>
