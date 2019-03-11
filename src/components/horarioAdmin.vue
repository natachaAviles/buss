<script>
import '@enrian/vue-pikaday'
import axios from 'axios'
export default {
  name: 'horarioAdmin',
  data() {
    return {
      horarios: [],
      trayectosDisp: [],
      piker: null,
      pikadayOptions: {
        format: 'MM/D/YYYY',
        inputValue: null,
      },
    }
  },
  mounted() {
    this.getHorarios()
    this.getTrayectos()
  },
  methods: {
    getHorarios() {
      axios
        .post(
          'http://ec2-18-231-137-134.sa-east-1.compute.amazonaws.com:8080/horario/all',
          { user: 'natacha_aviles', pass: 'destacamenatacha' }
        )
        .then((response) => {
          console.log(response)
          this.horarios = response.data
        })
        .catch((error) => {
          console.log(error)
        })
    },
    guardarHorario() {
      axios
        .post(
          'http://ec2-18-231-137-134.sa-east-1.compute.amazonaws.com:8080/horario',
          {
            user: 'natacha_aviles',
            pass: 'destacamenatacha',
            id_trayecto: this.trayecto.id,
            id_bus: this.trayecto.id,
          }
        )
        .response((response) => {
          console.log(response)
        })
        .then((error) => {
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
          this.trayectosDisp = response.data
        })
        .then((error) => {
          console.log(error)
        })
    },
  },
}
</script>

<template>
  <div class="column is-12">
    <div class="card">
      <header class="card-header">
        <p class="card-header-title">Crear Horarios</p>
        <div class="content">
          <form action method="post">
            <div class="field is-horizontal">
              <div class="field-body">
                <div class="field">
                  <label for="apellido">Fecha</label>
                  <vue-pikaday
                    class="input is-primary is-rounded"
                    v-model="piker"
                    :options="pikadayOptions"
                    placeholder="Fecha ida"
                  />
                </div>
                <div class="field">
                  <label for="apellido">Hora</label>
                  <p class="control is-expanded has-icons-left">
                    <input class="input is-primary" type="text" placeholder="hora">
                    <span class="icon is-small is-left">
                      <i class="fas fa-user"></i>
                    </span>
                  </p>
                </div>
                <div class="field has-addons">
                  <div class="control">
                    <label>RUT</label>
                    <input class="input is-primary" type="text" placeholder="RUT">
                  </div>
                  <div class="control">
                    <a class="button is-primary is-rounded">Registrar</a>
                  </div>
                </div>
              </div>
            </div>
          </form>
        </div>
      </header>
    </div>
    <div class="card-content">
      <div class="card-table"></div>
    </div>
  </div>
</template>

<style lang="scss" module>
@import '@design';
.picker {
  display: block;
  border-radius: 2px;
  box-shadow: none;
}
</style>
