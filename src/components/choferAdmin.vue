<script>
import axios from 'axios'
export default {
  data() {
    return {
      conductores: [],
      conductor: {
        nombre: '',
        apellido: '',
        rut: '',
      },
      edit: {
        nombre: '',
        apellido: '',
        rut: '',
      },
      chofer_id: '',
    }
  },
  mounted() {
    this.mostrarConductores()
  },
  methods: {
    show() {
      this.$modal.show('editConductor')
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
    crearConductor() {
      if (this.nombre && this.apellido && this.rut == '') {
        console.log('agregue los campos obligatorios')
      } else {
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
            this.mostrarConductores()
          })
          .catch((error) => {
            console.log(error)
          })
      }
    },
    editarConductor(id) {
      this.chofer_id = id
      console.log('editar este usuario' + id)
      this.show()
    },
    saveEdit() {
      axios
        .put(
          'http://ec2-18-231-137-134.sa-east-1.compute.amazonaws.com:8080/chofer/' +
            this.chofer_id,
          {
            user: 'natacha_aviles',
            pass: 'destacamenatacha',
            nombre: this.edit.nombre,
            apellido: this.edit.apellido,
            rut: this.edit.rut,
          }
        )
        .then((response) => {
          console.log(response)
          this.$modal.hide('hello-world')
          this.$swal({
            title: 'Bien!',
            text: 'Actualización de chofer ah sido exitosa',
            icon: 'success',
            buttons: false,
          })
          this.mostrarConductores()
        })
        .catch((error) => {
          console.log(error)
        })
    },
    eliminarConductor(id) {
      this.$swal({
        title: '¿Estas seguro que deseas eliminar este registro ?',
        text: 'Una vez eliminado el dato no se podrá recuperar',
        icon: 'warning',
        buttons: true,
        dangerMode: true,
      }).then((willDelete) => {
        if (willDelete) {
          axios
            .delete(
              'http://ec2-18-231-137-134.sa-east-1.compute.amazonaws.com:8080/chofer/' +
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
              this.mostrarConductores()
            })
            .then((error) => {
              console.log(error)
            })
        } else {
          swal('Tu registro no ha sido eliminado!')
        }
      })
    },
  },
}
</script>

<template>
  <div>
    <div class="column is-12">
      <div class="card events-card">
        <header class="card-header">
          <p class="card-header-title">Ingresar nuevo conductor</p>
          <div class="content" :class="$style.cardIngreso">
            <form action method="post" @submit.prevent="crearConductor">
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
                      >
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
                      >
                      <span class="icon is-small is-left">
                        <i class="fas fa-user"></i>
                      </span>
                    </p>
                  </div>
                  <div class="field has-addons">
                    <div class="control">
                      <label>RUT</label>
                      <input class="input" type="text" v-model="conductor.rut" placeholder="RUT">
                    </div>
                    <div class="control">
                      <a
                        @click="crearConductor"
                        class="button is-primary is-rounded"
                        :class="$style.button_register"
                      >Registrar</a>
                    </div>
                  </div>
                </div>
              </div>
            </form>
          </div>
        </header>
        <header class="card-header">
          <p class="card-header-title">Conductores registrados</p>
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
                <tr
                  v-for="(conductor, index) in conductores"
                  v-bind:index="index"
                  :key="conductor.id"
                >
                  <td width="5%">
                    <img
                      src="../assets/images/avatar.png"
                      alt="conductor"
                      :class="$style.avatar_conductor"
                    >
                  </td>
                  <td>{{ conductor.nombre }}</td>

                  <td>{{ conductor.apellido }}</td>
                  <td>{{ conductor.rut }}</td>
                  <td>
                    <a @click="editarConductor(conductor.id)" class="button is-small is-primary">
                      Editar
                      <i class="fas fa-pencil-alt"></i>
                    </a>
                  </td>
                  <td>
                    <a
                      @click="eliminarConductor(conductor.id)"
                      class="button is-small is-rounded is-danger"
                    >
                      Eliminar
                      <i class="far fa-trash-alt"></i>
                    </a>
                  </td>
                </tr>
              </tbody>
            </table>
          </div>
        </div>
        <footer class="card-footer"></footer>
      </div>
    </div>
    <modal name="editConductor" classes="modal_edit">
      <div slot="top-right">
        <button @click="$modal.hide('foo')">x</button>
      </div>
      <label>Nombre</label>
      <input
        class="input"
        type="text"
        v-model="edit.nombre"
        placeholder="Nombre"
        :class="$style.chofer_input"
      >
      <label>Apellido</label>
      <input
        class="input"
        type="text"
        v-model="edit.apellido"
        placeholder="Apellido"
        :class="$style.chofer_input"
      >
      <label>RUT</label>
      <input
        class="input"
        type="text"
        v-model="edit.rut"
        placeholder="RUT"
        :class="$style.chofer_input"
      >
      <a class="button is-primary" @click="saveEdit">Modificar Conductor</a>
    </modal>
  </div>
</template>

<style lang="scss" module>
@import '@design';
.cardIngreso input {
  border: 1px solid rgba(95, 186, 125, 0.9);
  height: 40px;
  border-radius: 2px;
  box-shadow: none;
}
.cardIngreso label {
  color: rgba(95, 186, 125, 0.9);
  margin-bottom: 15px;
}
.cardIngreso input:focus {
  border: 1px solid rgba(95, 186, 125, 0.9);
  box-shadow: 0 5px 20px 0 rgba(0, 0, 0, 0.1);
}
.cardIngreso {
  padding: 1em;
}
.cardIngreso .button_register {
  margin-top: 20px;
  height: 40px;
}
.chofer_input {
  margin: 6px 0;
  border-radius: 2px;
  height: 40px;
  box-shadow: none;
}
.avatar_conductor {
  width: 30px;
}
</style>

<style>
.modal_edit {
  padding: 1.5em;
  background-color: #ffffff;
}
.modal .input {
  margin: 15px 0;
  border: 1px solid #3273dc;
}
</style>
