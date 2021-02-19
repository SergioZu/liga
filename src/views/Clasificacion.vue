<template>
  <h2> La Clasificacion de la liga:</h2>
  <div class="clasificacion">
    <table style="width:100%">
      <tr>
        <th>Nombre</th>
        <th>Puntos</th> 
      </tr>
      <tr v-for="equipo in liga" :key="equipo">
        <td @click="obtenerEquipo(equipo.name)"> {{ equipo.name }}</td>
        <td >{{ equipo.points }}</td>
      </tr>
    </table>
  </div>
  <div class="datosJugador" v-if="equipoActual!=''">
    <DatosEquipos v-bind:nombreEquipo="equipoActual"/>
  </div>
</template>

<script>
import axios from "axios";
import DatosEquipos from '@/components/DatosEquipos.vue'

export default {
    data() {
        return {
            liga: [],
            equipoActual:""
        }
    },
    mounted() {
      axios
        .get("http://localhost:3000/clubs")
        .then(response => {
          this.liga = response.data;
        })
        .catch(response=>alert("Error al recuperar datos "+response.status));;
    },
    components: {
    DatosEquipos
  },
  methods:{
    obtenerEquipo: function(equipoA){
      this.equipoActual=equipoA;
    }
  }
}
</script>

<style scoped>

.clasificacion{
  width: 30%;
  margin-left: 20%;
  position: absolute;
 
}
.datosJugador{
  width: 30%;
  margin-left: 60%;
}
table, th, td {
  border: 1px solid black;
  border-collapse: collapse;
}
th, td {
  padding: 15px;
}

</style>