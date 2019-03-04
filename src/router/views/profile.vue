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
  data() {
    return {
      conductores: [],
      conductor: {
        nombre: '',
        apellido: '',
        rut: '',
      },
    }
  },
  mounted() {
    this.getChoferes()
  },
  methods: {
    getChoferes() {
      var body = {
        user: 'natacha_aviles',
        pass: 'destacamenatacha',
      }
      axios
        .post(
          'http://ec2-18-231-137-134.sa-east-1.compute.amazonaws.com:8080/chofer/all',
          body
        )
        .then((response) => {
          console.log(response)
          this.conductores = response.data
        })
        .catch((error) => {
          console.log(error)
        })
    },
    crearConductor() {
      axios
        .post(
          'http://ec2-18-231-137-134.sa-east-1.compute.amazonaws.com:8080/chofer',
          {
            user: 'natacha_aviles',
            pass: 'destacamenatacha',
            nombre: this.conductor.nombre,
            apellido: this.conductor.apellido,
            rut: this.conductor.rut,
          }
        )
        .then((response) => {
          console.log(response)
          this.getChoferes()
        })
        .catch((error) => {
          console.log(error)
        })
    },
  },
}
</script>

<template>
  <Layout>
    <div class="column is-12">
      <div class="card">
        <header class="card-header">
          <p class="card-header-title">
            Ingresar chofer
          </p>
        </header>
        <div class="content">
          <form action="" method="post" @submit.prevent="crearConductor">
            <div class="field is-horizontal">
              <div class="field-body">
                <div class="field">
                  <label for="apellido">Nombre</label>
                  <p class="control is-expanded has-icons-left">
                    <input
                      class="input"
                      type="text"
                      v-model="conductor.nombre"
                      placeholder="Name"
                    />
                    <span class="icon is-small is-left">
                      <i class="fas fa-user"></i>
                    </span>
                  </p>
                </div>
                <div class="field">
                  <label for="apellido">Apellido</label>
                  <p class="control is-expanded has-icons-left">
                    <input
                      class="input"
                      type="text"
                      v-model="conductor.apellido"
                      placeholder="Name"
                    />
                    <span class="icon is-small is-left">
                      <i class="fas fa-user"></i>
                    </span>
                  </p>
                </div>
                <div class="field">
                  <label for="rut">RUT</label>
                  <p class="control is-expanded has-icons-left has-icons-right">
                    <input
                      class="input"
                      type="tel"
                      v-model="conductor.rut"
                      placeholder="RUT"
                    />
                  </p>
                </div>
              </div>
            </div>
            <a
              @click="crearConductor"
              class="button is-small is-rounded is-primary"
              >Guardar conductor</a
            >
          </form>
        </div>
      </div>
    </div>
    <div class="column is-6">
      <div class="card events-card">
        <header class="card-header">
          <p class="card-header-title">
            Events
          </p>
          <a href="#" class="card-header-icon" aria-label="more options">
            <span class="icon">
              <i class="fa fa-angle-down" aria-hidden="true"></i>
            </span>
          </a>
        </header>
        <div class="card-table">
          <div class="content">
            <table class="table is-fullwidth is-striped">
              <tbody>
                <tr v-for="conductor in conductores" :key="conductor.id">
                  <td width="5%"><i class="fa fa-bell-o"></i></td>
                  <td>{{ conductor.nombre }}</td>
                  <td>{{ conductor.apellido }}</td>
                  <td>{{ conductor.rut }}</td>
                  <td
                    ><a class="button is-small is-primary" href="#"
                      >Action</a
                    ></td
                  >
                  <td>
                    <a href="#" class="button is-small is-rounded is-danger"
                      >Action</a
                    >
                  </td>
                </tr>
              </tbody>
            </table>
          </div>
        </div>
        <footer class="card-footer">
          <a href="#" class="card-footer-item">View All</a>
        </footer>
      </div>
    </div>
  </Layout>
</template>

<style lang="scss" module>
@import '@design';
input {
  border: 1px solid rgba(95, 186, 125, 0.9);
}
input:focus {
  border: 1px solid rgba(95, 186, 125, 0.9);
  box-shadow: 0 5px 20px 0 rgba(0, 0, 0, 0.1);
}
</style>
