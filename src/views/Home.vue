<template>
  <div class="home">
    <img alt="Vue logo" src="../assets/LaLigaLogo.png">
    <Inicio msg="Temporada"/>
  </div>
</template>

<script>
// @ is an alias to /src
import Inicio from '@/components/Inicio.vue'

export default {
  name: 'Home',
  components: {
    Inicio
  }
}
</script>

<style scoped>
img{
  width: 50%;
}
</style>
