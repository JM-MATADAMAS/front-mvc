<template>
  <v-card color="#BBDEFB" elevation="0" width="500">
    <v-card-title>
      Iniciar Sesión
    </v-card-title>
    <v-card-text>
      <v-form ref="form" v-model="validForm">
        Correo
        <v-text-field v-model="email" placeholder="Escribe tu correo" type="email" :rules="correo" />
        Contraseña
        <v-text-field v-model="passwordUser" placeholder="Escribe tu contraseña" type="password" :rules="password" />
      </v-form>
    </v-card-text>
    <v-card-actions>
      <v-row>
        <v-col cols="6">
          <v-btn block elevation="0" color="success" @click="loginUser">
            <span style="text-transform: none; color: white;">
              Login
            </span>
          </v-btn>
        </v-col>
        <v-col cols="6">
          <v-btn block elevation="0" color="error" @click="signupUser">
            <span style="text-transform: none; color: white;">
              Sign Up
            </span>
          </v-btn>
        </v-col>
      </v-row>
    </v-card-actions>
  </v-card>
</template>

<script>
export default {
  data () {
    return {
      validForm: false,
      email: null,
      passwordUser: null,
      required: [
        v => !!v || 'Campo requerido'
      ],
      password: [
        v => (v && v.length > 6) || 'La contraseña debe tener más de 6 caracteres'
      ],
      correo: [
        v => /.+@.+\..+/.test(v) || 'Ingresa un correo válido' // Devuelve una cadena vacía si la dirección de correo electrónico es válida
      ]
    }
  },

  methods: {
    loginUser () {
      this.validForm = this.$refs.form.validate()
      if (this.validForm) {
        // Se puede logear
        const sendData = {
          email: this.email,
          password: this.passwordUser
        }
        const url = '/login'
        this.$axios.post(url, sendData)
          .then((res) => {
            // eslint-disable-next-line no-console
            console.log('@@ res =>', res)
            if (res.data.token) {
              localStorage.setItem('token', res.data.token)
              // Redirigir a una página nueva
              this.$router.push('/principal')
            }
          })
          .catch((err) => {
            // eslint-disable-next-line no-console
            console.log('@@ err =>', err)
          })
      } else {
        alert('Algo está mal')
      }
    },
    signupUser () {
      // Implementar la lógica para registrarse
    }
  }
}
</script>
