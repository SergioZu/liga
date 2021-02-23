<template>
<h2> Nuevo Jugador:</h2>
  <div class="nuevoPartidos">
    <form v-if="this.tipoVista=='nuevoJugador'">
      <label >Nombre del jugador:</label><br>
      <input type="text"  v-model="nombreJugador"><br>
        <label >Nombre del equipo:</label><br>
      <select name="select" v-model="equipoJugador">
        <option >...</option>
        <option  v-for="equipo in clubes" :key="equipo">{{equipo.name}}</option>
      </select><br>
      <label >Goles del Jugador:</label><br>
       <input type="text" v-model="scoreJugador"><br>
      <button type="button"  @click="crearJugador()">enviar</button>
    </form>
    <form v-if="this.tipoVista=='equiposV'">
      <label >Nombre del jugador:</label><br>
      <input type="text"  v-model="nombreJugador"><br>
      <label >Equipo del jugador:</label><br>
       <label >{{this.nombreEquipo}}</label><br>
      <label >Goles del Jugador:</label><br>
       <input type="text" v-model="scoreJugador"><br>
      <button type="button"  @click="crearJugador()">enviar</button>
    </form>
  </div>
</template>

<script>
import axios from "axios";

export default {
  props:['tipoVista','nombreEquipo'],
  data() {
        return {
            clubes: [],
            nombreJugador:"",
            nombreE:"",
            scoreJugador:""
        }
    },
  
   mounted() {
        axios
        .get("http://localhost:3000/clubs")
        .then(response => {
          this.clubes = response.data;
        })
        .catch(response=>alert("Error al recuperar datos "+response.status))
    },
    methods:{
      crearJugador: function(){
        let players={
          name:this.nombreJugador,
          team:this.nombreEquipo,
          scores:this.scoreJugador
        }
        axios.post("http://localhost:3000/players", players).then((result) => {
            console.log(result);
            alert("Se ha insertado el jugador Correctamente");
            });
        }
    }
  
}
</script>

<style>

.nuevoPartidos{
  width: 100%;
}

</style>
