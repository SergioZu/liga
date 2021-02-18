<template>
  <div class="jugadoresDiv">
    <ul>
        <li>Nombre:          Puntos:</li>
        <li v-for="jugador in jugadores" :key="jugador">{{jugador.name}}      {{jugador.scores}}</li>
    </ul>  
  </div>
</template>

<script>
import axios from "axios";


export default {
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
    }
}
</script>

<style >

.jugadoresDiv{
  width: 60%;
  margin-left: 100px;
}
</style>