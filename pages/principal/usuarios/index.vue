<template>
  <v-col cols="20">
    <v-row>
      <v-btn block color="success" @click="showNuevo = true">
        <span style="color: white; text-transform: none;">
          Usuario Nuevo
        </span>
      </v-btn>
    </v-row>
    <v-row class="mt-3">
      <v-data-table :headers="headers" :items="usuarios" elevation="0" style="width: 100% !important;">
        <template #[`item.acciones`]="{ item }">
          <v-row>
            <v-col cols="6">
              <v-btn icon color="error" @click="borrarUsuario(item.id)">
                <v-icon>
                  mdi-account-minus
                </v-icon>
              </v-btn>
            </v-col>
            <v-col cols="6">
              <v-btn icon color="warning" @click="actualizarUsuario(item)">
                <v-icon>
                  mdi-account-eye
                </v-icon>
              </v-btn>
            </v-col>
          </v-row>
        </template>
      </v-data-table>
    </v-row>
    <v-dialog v-model="showDelete" width="400" persistent>
      <v-card>
        <v-card-title>Usuarios</v-card-title>
        <v-card-text>Are you sure?</v-card-text>
        <v-card-actions>
          <v-row>
            <v-col cols="6">
              <v-btn block color="error" @click="borrar">
                <span style="color: white; text-transform: none;">
                  Borrar
                </span>
              </v-btn>
            </v-col>
            <v-col cols="6">
              <v-btn block color="success" @click="showDelete = false">
                <span style="color: white; text-transform: none;">
                  Cancelar
                </span>
              </v-btn>
            </v-col>
          </v-row>
        </v-card-actions>
      </v-card>
    </v-dialog>
    <v-dialog v-model="showNuevo" width="500" persistent>
      <v-card>
        <v-card-title>Registrar usuario</v-card-title>
        <v-card-text>
          <v-form ref="form">
            <v-row>
              <v-col cols="6">
                Nombre
                <v-text-field v-model="nombreNuevo" placeholder="Escribe tu nombre" type="text" :rules="required" />
              </v-col>
              <v-col cols="6">
                Apellido Paterno
                <v-text-field v-model="apaternoNuevo" placeholder="Escribe tu apellido paterno" type="text" :rules="required" />
              </v-col>
              <v-col cols="6">
                Apellido Materno
                <v-text-field v-model="amaternoNuevo" placeholder="Escribe tu apellido materno" type="text" :rules="required" />
              </v-col>
              <v-col cols="6">
                Direccion
                <v-text-field v-model="direccionNuevo" placeholder="Escribe tu direccion" type="text" :rules="required" />
              </v-col>
              <v-col cols="6">
                Telefono
                <v-text-field v-model="telefonoNuevo" placeholder="Escribe tu telefono" type="number" :rules="telefono" />
              </v-col>
              <v-col cols="6">
                Estado
                <v-select v-model="estadoNuevo" :items="estados" placeholder="ingresa tu estado" :rules="required" />
              </v-col>
              <v-col cols="6">
                Correo
                <v-text-field v-model="email" placeholder="Escribe tu correo" type="email" :rules="correo" />
              </v-col>
              <v-col cols="6">
                Contraseña
                <v-text-field
                  v-model="passwordUser"
                  placeholder="Escribe tu contraseña"
                  type="password"
                  :rules="password"
                />
              </v-col>
            </v-row>
          </v-form>
        </v-card-text>
        <v-card-actions>
          <v-row>
            <v-col cols="6">
              <v-btn block color="success" @click="agregar">
                <span style="color: white; text-transform: none;">
                  Agregar
                </span>
              </v-btn>
            </v-col>
            <v-col cols="6">
              <v-btn block color="error" @click="showNuevo = false">
                <span style="color: white; text-transform: none;">
                  Cancelar
                </span>
              </v-btn>
            </v-col>
          </v-row>
        </v-card-actions>
      </v-card>
    </v-dialog>
    <v-dialog v-model="showUpdate" width="400" persistent>
      <v-card>
        <v-card-title>Modificar usuario</v-card-title>
        <v-card-text>
          <v-form ref="formUpdate" v-model="validFormUpdate">
            <v-row>
              <v-col cols="6">
                Nombre
                <v-text-field
                  v-model="userToUpdate.nombre"
                  placeholder="Escribe tu nombre"
                  type="text"
                  :rules="required"
                />
              </v-col>
              <v-col cols="6">
                Apellido Paterno
                <v-text-field
                  v-model="userToUpdate.apaterno"
                  placeholder="Escribe tu apellido paterno"
                  type="text"
                  :rules="required"
                />
              </v-col>
              <v-col cols="6">
                Apellido Materno
                <v-text-field
                  v-model="userToUpdate.amaterno"
                  placeholder="Escribe tu apellido materno"
                  type="text"
                  :rules="required"
                />
              </v-col>
              <v-col cols="6">
                Direccion
                <v-text-field
                  v-model="userToUpdate.direccion"
                  placeholder="Escribe tu direccion"
                  type="text"
                  :rules="required"
                />
              </v-col>
              <v-col cols="6">
                Telefono
                <v-text-field
                  v-model="userToUpdate.telefono"
                  placeholder="Escribe tu telefono"
                  type="number"
                  :rules="telefono"
                />
              </v-col>
              <v-col cols="6">
                Estado
                <v-select
                  v-model="userToUpdate.estado"
                  :items="estados"
                  placeholder="ingresa tu estado"
                  :rules="required"
                />
              </v-col>
              <v-col cols="6">
                Correo
                <v-text-field
                  v-model="userToUpdate.email"
                  placeholder="Escribe tu correo"
                  type="email"
                  :rules="correo"
                />
              </v-col>
              <v-col cols="6">
                Contraseña
                <v-text-field
                  v-model="userToUpdate.password"
                  placeholder="Escribe tu contraseña"
                  type="password"
                  :rules="password"
                />
              </v-col>
            </v-row>
          </v-form>
        </v-card-text>
        <v-card-actions>
          <v-row>
            <v-col cols="6">
              <v-btn block color="success" @click="modificar">
                <span style="color: white; text-transform: none;">
                  Modificar
                </span>
              </v-btn>
            </v-col>
            <v-col cols="6">
              <v-btn block color="error" @click="showUpdate = false">
                <span style="color: white; text-transform: none;">
                  Cancelar
                </span>
              </v-btn>
            </v-col>
          </v-row>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </v-col>
</template>

<script>
export default {
  data () {
    return {
      headers: [
        {
          text: 'ID',
          align: 'center',
          sortable: true,
          value: 'id'
        },
        {
          text: 'NOMBRE',
          align: 'center',
          sortable: true,
          value: 'nombre'
        },
        {
          text: 'APELLIDO PATERNO',
          align: 'center',
          sortable: true,
          value: 'apaterno'
        },
        {
          text: 'APELLIDO MATERNO',
          align: 'center',
          sortable: true,
          value: 'amaterno'
        },
        {
          text: 'DIRECCIÓN',
          align: 'center',
          sortable: false,
          value: 'direccion'
        },
        {
          text: 'TELÉFONO',
          align: 'center',
          sortable: true,
          value: 'telefono'
        },
        {
          text: 'ESTADO',
          align: 'center',
          sortable: true,
          value: 'estado'
        },
        {
          text: 'E-MAIL',
          align: 'center',
          sortable: true,
          value: 'email'
        },
        {
          text: 'ACTIONS CRUD',
          align: 'center',
          sortable: false,
          value: 'acciones'
        }
      ],
      token: null,
      usuarios: [],
      idToDelete: null,
      showDelete: false,
      showNuevo: false,
      showUpdate: false,
      validForm: false,
      validFormUpdate: false,
      email: null,
      passwordUser: null,
      nombreNuevo: null,
      apaternoNuevo: null,
      amaternoNuevo: null,
      direccionNuevo: null,
      telefonoNuevo: null,
      estadoNuevo: null,
      estados: ['Aguascalientes', 'Baja California', 'Baja California Sur', 'Campeche', 'Chiapas', 'Chihuahua', 'Ciudad de México', 'Coahuila', 'Colima', 'Durango', 'Guanajuato', 'Guerrero', 'Hidalgo', 'Jalisco', 'Estado de México', 'Michoacán', 'Morelos', 'Nayarit', 'Nuevo León', 'Oaxaca', 'Puebla', 'Querétaro', 'Quintana Roo', 'San Luis Potosí', 'Sinaloa', 'Sonora', 'Tabasco', 'Tamaulipas', 'Tlaxcala', 'Veracruz', 'Yucatán', 'Zacatecas'],
      userToUpdate: {},
      required: [
        v => !!v || 'Campo requerido'
      ],
      telefono: [
        v => (v && v.length === 10) || 'El numero de telefono debe poseer 10 digitos'
      ],
      password: [
        v => (v && v.length > 6) || 'La contraseña debe tener más de 6 caracteres'
      ],
      correo: [
        v => /.+@.+\..+/.test(v) || 'Ingresa un correo válido' // Devuelve una cadena vacía si la dirección de correo electrónico es válida
      ]
    }
  },
  mounted () {
    this.token = localStorage.getItem('token')
    if (!this.token) {
      this.$router.push('/')
    }
    this.getAllUsers()
  },
  methods: {
    getAllUsers () {
      const url = '/get-allusers'
      const config = {
        headers: {
          Authorization: `Bearer ${this.token}`
        }
      }
      this.$axios.get(url, config)
        .then((res) => {
          // eslint-disable-next-line no-console
          console.log('@@@ res => ', res)
          if (res.data.message === 'Success') {
            this.usuarios = res.data.users
          } else if (res.data.message === 'Invalid Token') {
            this.$router.push('/')
          }
        })
        .catch((err) => {
          // eslint-disable-next-line no-console
          console.log('@@@ err => ', err)
        })
    },
    borrarUsuario (id) {
      this.idToDelete = id
      this.showDelete = true
    },
    borrar () {
      const url = `/delete-user/${this.idToDelete}`
      const config = {
        headers: {
          authorization: `Bearer ${this.token}`
        }
      }
      this.$axios.delete(url, config)
        .then((res) => {
          // eslint-disable-next-line no-console
          console.log('@@@ res => ', res)
          if (res.data.message === 'User Deleted Successfully') {
            this.$nuxt.$emit('evento', {
              message: res.data.message,
              color: 'error',
              type: 'error'
            })
            this.showDelete = false
            this.getAllUsers()
            this.usuarios = res.data.users
          } else if (res.data.message === 'Invalid Token') {
            this.$router.push('/')
          }
        })
        .catch((err) => {
          // eslint-disable-next-line no-console
          console.log('@@@ err => ', err)
        })
    },
    agregar () {
      this.validForm = this.$refs.form.validate()
      if (this.validForm) {
        const sendData = {
          id: Date.now().toString(),
          nombre: this.nombreNuevo,
          apaterno: this.apaternoNuevo,
          amaterno: this.amaternoNuevo,
          direccion: this.direccionNuevo,
          telefono: this.telefonoNuevo,
          estado: this.estadoNuevo,
          email: this.email,
          password: this.passwordUser
        }
        // eslint-disable-next-line no-console
        console.log('@@ data =>', sendData)
        const url = '/signup'
        this.$axios.post(url, sendData)
          .then((res) => {
            // eslint-disable-next-line no-console
            console.log('@@ res =>', res)
            if (res.data.message === 'Usuario registrado satisfactoriamente') {
              this.$nuxt.$emit('evento', {
                message: res.data.message,
                color: 'success',
                type: 'success',
                time: 2000
              })
              this.getAllUsers()
              this.showNuevo = false
              this.email = ''
              this.passwordUser = ''
            }
          })
          .catch((err) => {
            // eslint-disable-next-line no-console
            console.log('@@ err =>', err)
          })
      } else {
        alert('Faltan datos')
      }
    },
    actualizarUsuario (user) {
      this.userToUpdate = user
      this.showUpdate = true
    },
    modificar () {
      this.validFormUpdate = this.$refs.formUpdate.validate()
      if (this.validFormUpdate) {
        const sendData = {
          id: this.userToUpdate.id,
          email: this.userToUpdate.email,
          password: this.userToUpdate.password
        }
        this.$axios.defaults.headers.common.Authorization = `Bearer ${this.token}`

        // eslint-disable-next-line no-console
        console.log('@@ data =>', sendData)
        const url = `/update-user/${sendData.id}`
        this.$axios.put(url, sendData)
          .then((res) => {
            // eslint-disable-next-line no-console
            console.log('@@ res =>', res)
            if (res.data.message === 'User Updated Successfully') {
              this.$nuxt.$emit('evento', {
                message: res.data.message,
                color: 'warning',
                type: 'success',
                time: 3000
              })
              this.getAllUsers()
              this.showUpdate = false
              this.email = ''
              this.passwordUser = ''
            }
          })
          .catch((err) => {
            // eslint-disable-next-line no-console
            console.log('@@ err =>', err)
          })
      } else {
        alert('Faltan datos')
      }
    }
  }
}
</script>
