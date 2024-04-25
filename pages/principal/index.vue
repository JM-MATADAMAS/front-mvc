<template>
  <div>
    Hola a todos
  </div>
</template>

<script>
export default {
  mounted () {
    const token = localStorage.getItem('token')
    if (!token) {
      this.$router.push('/')
    }
  }
}
</script>
