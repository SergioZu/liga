<template>
  <div class="jugadoresDiv">
    <ul>
        <li>Nombre:</li>
        <span v-for="jugador in jugadores" :key="jugador" >
          <li  v-if="this.nombreEquipo==jugador.team" @click="masInformacion(jugador)" >{{jugador.name}}</li>
        </span>
        
    </ul>  
  </div>

  <div v-if="nombreJugador!=''" class="datosJugadores">
        <ul>
        <li>Datos: </li>
        <span >
          <li   v-if="nombreJugador==this.jugadorObjeto.name" >{{jugadorObjeto.name}}</li>
            <li   v-if="nombreJugador==this.jugadorObjeto.name" >{{jugadorObjeto.team}}</li>
              <li   v-if="nombreJugador==this.jugadorObjeto.name" >
                  <input type="number" v-model="scoreNuevo">
              </li>
        </span>
        <button type="button"  @click="sumarPuntos()">Sumar</button>
        <button type="button"  @click="eliminarJugador()">Eliminar</button>
    </ul>  
  </div>

</template>

<script>
import axios from "axios";


export default {
  props:['nombreEquipo'],

    data() {
        return {
            jugadores: [],
            jugadorObjeto:"",
            nombreJugador:"",
            scoreAux:0,
            scoreNuevo:""
        }
    },
    mounted() {
      axios
        .get("http://localhost:3000/players/")
        .then(response => {
          this.jugadores = response.data;
        })
        .catch(response=>alert("Error al recuperar datos "+response.status));;
    }, methods:{
        masInformacion: function(jugador){
            this.jugadorObjeto=jugador;
            this.nombreJugador=jugador.name;
        },
        sumarPuntos: function(){
          this.scoreNuevo=parseInt(this.jugadorObjeto.scores)+parseInt(this.scoreNuevo);
        let players={
          id:this.jugadorObjeto.id,
          name:this.jugadorObjeto.name,
          team:this.jugadorObjeto.team,
          scores: this.scoreNuevo
        }
      

        axios.put("http://localhost:3000/players/"+this.jugadorObjeto.id, players).then((result) => {
          
          alert("Se ha sumado los goles nuevos")
        }); 
    },eliminarJugador: function(){
        axios.delete("http://localhost:3000/players/"+this.jugadorObjeto.id).then((result) => {
          
          alert("Se ha eliminado el Jugador")
        }); 
    }
  }
}
</script>

<style >

.jugadoresDiv{
  width: 60%;
  margin-left: 100px;
}

.datosJugadores{
     width: 60%;
  margin-left: 100px;
  margin-top: 300px;
}
</style>