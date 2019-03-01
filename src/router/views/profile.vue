<script>
import Layout from '@layouts/main'
import axios from 'axios'

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
  components: { Layout },
  props: {
    user: {
      type: Object,
      required: true,
    },
  },
  mounted() {
    this.getChoferes()
  },
  methods: {
    getChoferes() {
      var headers = {
        'Content-Type': 'application/x-www-form-urlencoded',
        'Access-Control-Allow-Origin': '*',
      }
      var body = {
        user: 'natacha_aviles',
        pass: 'destacamenatacha',
      }
      axios
        .post(
          'http://ec2-18-231-137-134.sa-east-1.compute.amazonaws.com:8080/chofer/all',
          body,
          { headers: headers }
        )
        .then((response) => {
          // console.log(response)
        })
      /* .catch((error) => {
          // console.log(error)
        }) */
    },
  },
}
</script>

<template>
  <Layout>
    <h1>
      <BaseIcon name="user" />
      {{ user.name }}
      Profile
    </h1>
    <pre>{{ user }}</pre>
  </Layout>
</template>
