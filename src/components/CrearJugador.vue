<template>
  <div class="jugadoresDiv">
      <h2>{{this.nombreEquipo}}</h2>
      <ul>
          <li>Nombre:</li>
          <span v-for="jugador in jugadores" :key="jugador" >
            <li   v-if="this.nombreEquipo==jugador.team">{{jugador.name}}</li>
          </span>
      </ul>  
      <button type="button"  @click="crearJugador()">enviar</button>
  </div>
</template>

<script>
import axios from "axios";


export default {
  props:['nombreEquipo'],

    data() {
        return {
            jugadores: []
        }
    },
    mounted() {
      axios
        .get("http://localhost:3000/players/")
        .then(response => {
          this.jugadores = response.data;
        })
        .catch(response=>alert("Error al recuperar datos "+response.status));;
    },
    methods:{
      crearJornada: function(){
        let matches={
          round:this.numerojornada,
          date:this.fechaJornada,
          team1:this.equipo1,
          team2:this.equipo2
        }
        axios.post("http://localhost:3000/matches", matches).then((result) => {
            console.log(result);
            alert("Se ha insertado la Jornada Correctamente");
            });
        }
    }
}
</script>

<style >

.jugadoresDiv{
  width: 50%;
}
</style>