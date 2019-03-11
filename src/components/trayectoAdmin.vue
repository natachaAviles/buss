<script>
import axios from 'axios'
export default {
  data() {
    return {
      trayecto: {
        ida: '',
        vuelta: '',
        terminal: '',
      },
      trayectos: [],
    }
  },
  mounted() {
    this.getTrayectos()
  },
  methods: {
    guardarTrayecto() {
      axios
        .post(
          'http://ec2-18-231-137-134.sa-east-1.compute.amazonaws.com:8080/trayecto',
          {
            user: 'natacha_aviles',
            pass: 'destacamenatacha',
            ida: this.trayecto.ida,
            vuelta: this.trayecto.vuelta,
            terminal: this.trayecto.terminal,
          }
        )
        .then((response) => {
          console.log(response)
          this.getTrayectos()
          this.$swal({
            title: 'Bien!',
            text: 'Se ha guardado el trayecto con exito!.',
            icon: 'success',
            buttons: false,
          })
        })
        .catch((error) => {
          console.log(error)
        })
    },
    getTrayectos() {
      axios
        .post(
          'http://ec2-18-231-137-134.sa-east-1.compute.amazonaws.com:8080/trayecto/all',
          { user: 'natacha_aviles', pass: 'destacamenatacha' }
        )
        .then((response) => {
          console.log(response)
          this.trayectos = response.data
        })
        .catch((error) => {
          console.log(error)
        })
    },
    editarTrayecto() {
      axios
        .put(
          'http://ec2-18-231-137-134.sa-east-1.compute.amazonaws.com:8080/trayecto/' +
            id,
          { user: 'natacha_aviles', pass: 'destacamenatacha' }
        )
        .then((response) => {
          console.log(response)
        })
        .catch((error) => {
          console.log(error)
        })
    },
  },
}
</script>

<template>
  <div class="column is-12">
    <div class="card">
      <div class="card-header">
        <p class="card-header-title">Registrar nuevo trayecto</p>
      </div>
      <div class="card-content">
        <div class="field is-horizontal">
          <div class="field-body">
            <div class="field">
              <p class="control is-expanded has-icons-left">
                <input class="input" type="text" placeholder="Ida" v-model="trayecto.ida">
                <span class="icon is-small is-left">
                  <i class="fas fa-user"></i>
                </span>
              </p>
            </div>
            <div class="field">
              <p class="control is-expanded has-icons-left">
                <input class="input" type="text" placeholder="Vuelta" v-model="trayecto.vuelta">
                <span class="icon is-small is-left">
                  <i class="fas fa-user"></i>
                </span>
              </p>
            </div>
            <div class="field">
              <p class="control is-expanded has-icons-left">
                <input class="input" type="text" placeholder="Terminal" v-model="trayecto.terminal">
                <span class="icon is-small is-left">
                  <i class="fas fa-user"></i>
                </span>
              </p>
            </div>
          </div>
        </div>
        <a class="button is-primary" @click="guardarTrayecto">AGREGAR TRAYECTO</a>
        <div class="card-table">
          <div class="content">
            <table class="table is-fullwidth is-striped">
              <tr>
                <td>Ida</td>
                <td>Vuelta</td>
                <td>Terminal</td>
              </tr>
              <tr v-for="(trayecto,index) in trayectos" v-bind:index="index" :key="trayecto.id">
                <td>{{trayecto.ida}}</td>
                <td>{{trayecto.vuelta}}</td>
                <td>{{trayecto.terminal}}</td>
                <td>
                  <a class="button is-small is-success">Editar</a>
                  <a class="button is-small is-danger">Eliminar</a>
                </td>
              </tr>
            </table>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style lang="scss" module>
@import '@design';
</style>
