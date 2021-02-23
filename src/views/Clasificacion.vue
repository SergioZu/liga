<template>
  <h2> La Clasificacion de la liga:</h2>
  <div class="clasificacion">
    <table style="width:100%">
      <tr>
        <th>Nombre</th>
        <th>Puntos</th> 
      </tr>
      <tr v-for="equipo in clubs" :key="equipo">
        <td @click="obtenerEquipo(equipo.name)"> {{ equipo.name }}</td>
        <td>{{ equipo.points }}</td>
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
            clubs: [],
            clubsOrdenados:[],
            jornadas:[],
            puntos:[],
            punto:0,
            equipoActual:""
        }
    },
    components: {
    DatosEquipos
  },
  methods:{
    obtenerEquipo: function(equipoA){
      this.equipoActual=equipoA;
    },
    obtenerClubs: function(){
      axios
        .get("http://localhost:3000/clubs")
        .then(response => {
          this.clubs = response.data;
          this.ordenarAsc( this.clubs,"points");
        })
        .catch(response=>alert("Error al recuperar datos "+response.status));
      },
       ordenarAsc: function(array, etiqueta) {
        array.sort(function (a, b) {
            return a[etiqueta] < b[etiqueta];
        });
      }
  },
   created(){
     this.obtenerClubs();
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