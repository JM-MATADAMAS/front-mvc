<template>
  <v-app>
    <v-main>
      <ui-alert
        v-if="showAlert"
        :message="mensaje"
        :color="color"
        :type="type"
      />
      <v-row style="width: 100; height: 100%;" justify="center" align="center">
        <nuxt />
      </v-row>
    </v-main>
  </v-app>
</template>

<script>
export default {
  data () {
    return {
      showAlert: false,
      mensaje: '',
      color: '',
      type: ''
    }
  },
  created () {
    this.$nuxt.$on('evento', (data) => {
      // eslint-disable-next-line no-console
      console.log('@@@ data login =>', data)
      this.mensaje = data.message
      this.color = data.color
      this.type = data.type
      this.showAlert = true
      setTimeout(() => {
        this.showAlert = false
      }, 5000)
    })
  }
}
</script>
