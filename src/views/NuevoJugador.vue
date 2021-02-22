<template>
  <div class="clasificacion">
    <FormularioJugador :tipoVista="'nuevoJugador'"/>
  </div>
</template>

<script>
import FormularioJugador from '@/components/FormularioJugador.vue'

export default {
   
    components: {
    FormularioJugador
  }
}
</script>

<style scoped>

.clasificacion{
  width: 30%;
  margin-left: 40%;
  position: absolute;
 
}


</style>