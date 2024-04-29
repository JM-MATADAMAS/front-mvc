<template>
  <v-alert
    dense
    :type="type"
    :color="color"
    :dismissible="dismissible"
    class="alertClass"
  >
    {{ message }}
  </v-alert>
</template>

<script>
export default {
  props: {
    message: { type: String, default: '' },
    dismissible: { type: Boolean, default: true },
    color: { type: String, default: 'primary' },
    type: { type: String, default: 'success' }
  }
}
</script>

<style scoped>
.alertClass {
  position: absolute !important;
  width: 500px !important;
  right: 10px !important;
  top: 10px !important;
}
</style>
