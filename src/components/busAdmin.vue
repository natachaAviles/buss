<script>
import axios from 'axios'
export default {
  data() {
    return {
      conductores: [],
      bus: {
        patente: '',
        marca: '',
        chofer_id: 0,
      },
      edit: {
        patente: '',
        marca: '',
        chofer: '',
      },
      bus_id: '',
      buses: [],
    }
  },
  mounted() {
    this.mostrarConductores()
    this.mostrarBuses()
  },
  methods: {
    crearBus() {
      axios
        .post(
          'http://ec2-18-231-137-134.sa-east-1.compute.amazonaws.com:8080/bus',
          {
            user: 'natacha_aviles',
            pass: 'destacamenatacha',
            patente: this.bus.patente,
            marca: this.bus.marca,
            id_chofer: this.bus.chofer_id,
          }
        )
        .then((response) => {
          console.log(response)
          this.$swal({
            title: 'Bien!',
            text: 'Bus creado exitosamente',
            icon: 'success',
            button: false,
          })
        })
        .catch((error) => {
          console.log(error)
        })
    },
    editBus(id) {
      console.log(id)
      this.bus_id = id
      this.$modal.show('ModalBusEdit')
    },
    SaveEditBus(id) {
      axios
        .put(
          'http://ec2-18-231-137-134.sa-east-1.compute.amazonaws.com:8080/bus/' +
            this.bus_id,
          {
            user: 'natacha_aviles',
            pass: 'destacamenatacha',
            patente: this.edit.patente,
            marca: this.edit.marca,
            id_chofer: this.edit.chofer,
          }
        )
        .then((response) => {
          console.log(response)
          this.mostrarBuses()
          this.$modal.hide('ModalBusEdit')
        })
        .catch((error) => {
          console.log(error)
        })
    },
    deleteBus(id) {
      axios
        .delete(
          'http://ec2-18-231-137-134.sa-east-1.compute.amazonaws.com:8080/bus/' +
            id,
          {
            data: {
              user: 'natacha_aviles',
              pass: 'destacamenatacha',
            },
          }
        )
        .then((response) => {
          console.log(response)
          this.mostrarBuses()
        })
        .catch((error) => {
          console.log(error)
        })
    },
    mostrarConductores() {
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
    mostrarBuses() {
      axios
        .post(
          'http://ec2-18-231-137-134.sa-east-1.compute.amazonaws.com:8080/bus/all',
          {
            user: 'natacha_aviles',
            pass: 'destacamenatacha',
          }
        )
        .then((response) => {
          console.log(response)
          this.buses = response.data
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
      <header class="card-header"></header>
      <table class="table is-fullwidth is-striped">
        <tr>
          <td>Patente</td>
          <td>Marca</td>
          <td>Conductores Registrados</td>
          <td></td>
          <td></td>
          <td></td>
        </tr>
        <tr>
          <td>
            <input type="text" class="input is-primary" v-model="bus.patente">
          </td>
          <td>
            <input type="text" class="input is-primary" v-model="bus.marca">
          </td>
          <td>
            <div class="select is-primary">
              <select class="select is-primary" v-model="bus.chofer_id">
                <option
                  v-for="(conductor,index) in conductores"
                  :value="conductor.id"
                >{{conductor.nombre}}</option>
              </select>
            </div>
          </td>
          <td>
            <a class="button is-rounded is-info" @click="crearBus">Guardar Bus</a>
          </td>
          <td></td>
          <td></td>
        </tr>
        <tr v-for="(bus,index) in buses" :key="bus.id" :index="index">
          <td>{{bus.patente}}</td>
          <td>{{bus.marca}}</td>
          <td>{{bus.id_chofer}}</td>
          <td>
            <a class="button is-small is-info">Designar trayecto</a>
          </td>
          <td>
            <a class="button is-small is-success" @click="editBus(bus.id)">Editar</a>
          </td>
          <td>
            <a class="button is-small is-danger" @click="deleteBus(bus.id)">Eliminar</a>
          </td>
        </tr>
      </table>
      <modal name="ModalBusEdit" classes="modal_edit">
        <div slot="top-right">
          <button @click="$modal.hide('foo')">x</button>
        </div>
        <label>Patente</label>
        <input class="input" type="text" v-model="edit.patente" placeholder="Name">
        <label>Marca></label>
        <input class="input" type="text" v-model="edit.marca" placeholder="Name">
        <label>Conductor</label>
        <input class="input" type="text" v-model="edit.chofer" placeholder="Name">
        <a class="is-link is-outlined" @click="SaveEditBus">Modificar Conductor</a>
      </modal>
    </div>
  </div>
</template>

<style lang="scss" module>
@import '@design';
</style>
