<template>
  <div>
    <v-col cols="12">
      <v-row>
        <v-btn block color="success" @click="showNuevo = true">
          <span style="color: white; text-transform: none;">
            Usuario Nuevo
          </span>
        </v-btn>
      </v-row>
      <v-row class="mt-3">
        <v-data-table
          :headers="headers"
          :items="usuarios"
          elevation="0"
          style="width: 100% !important;"
        >
          <template #[`item.acciones`]="{item}">
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
      <v-dialog v-model="showNuevo" width="400" persistent>
        <v-card>
          <v-card-title>Registrar usuario</v-card-title>
          <v-card-text>
            <v-form ref="form">
              Correo
              <v-text-field v-model="email" placeholder="Escribe tu correo" type="email" :rules="correo" />
              Contraseña
              <v-text-field v-model="passwordUser" placeholder="Escribe tu contraseña" type="password" :rules="password" />
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
                <v-btn block color="error" @click="showNuevo= false">
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
            <v-form
              ref="formUpdate"
              v-model="validFormUpdate"
            >
              Correo
              <v-text-field v-model="userToUpdate.email" placeholder="Escribe tu correo" type="email" :rules="correo" />
              Contraseña
              <v-text-field v-model="userToUpdate.passwordUser" placeholder="Escribe tu contraseña" type="password" :rules="password" />
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
                <v-btn block color="error" @click="showUpdate= false">
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
  </div>
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
      userToUpdate: {},
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
