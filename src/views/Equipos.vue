<template>
  <h2> Equipos de la liga:</h2>
  <div class="clasificacion">
    <table style="width:100%">
      <tr>
        <th>Nombre</th> 
      </tr>
      <tr v-for="equipo in equipos" :key="equipo">
        <td @click="obtenerEquipo(equipo.name)"> 
          {{ equipo.name }}
           <span v-for="jugador in jugadores" :key="jugador">
                <p v-if="jugador.team==equipo.name" @click="obtenerEquipo(jugador.name)">{{jugador.name}}</p>
            </span>  
        </td>
      </tr>
    </table>
     <button type="button" @click="activarComponente()">Nuevo Jugador</button>
  </div>

    <div class="formularioJugador" v-if="activado!=false">
        <FormularioJugador :tipoVista="'equiposV'" :equipoJugador="equipoActual"/>
    </div>
 
</template>

<script>
import axios from "axios";
import FormularioJugador from '@/components/FormularioJugador.vue'
export default {
    data() {
        return {
            equipos: [],
            jugadores:[],
            equipoActual:"",
            activado:false
        }
    },
    mounted() {
      axios
        .get("http://localhost:3000/clubs")
        .then(response => {
          this.equipos = response.data;
        })
        .catch(response=>alert("Error al recuperar datos "+response.status));

        axios
        .get("http://localhost:3000/players")
        .then(response => {
          this.jugadores = response.data;
        })
        .catch(response=>alert("Error al recuperar datos "+response.status));
    },
  methods:{
    obtenerEquipo: function(equipoA){
      this.equipoActual=equipoA;
    },
    activarComponente: function(){
      this.activado=true;
    }
  },
    components: {
    FormularioJugador
  },
}
</script>

<style scoped>

.clasificacion{
  width: 30%;
  margin-left: 20%;
  position: absolute;
 
}
.formularioJugador{
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