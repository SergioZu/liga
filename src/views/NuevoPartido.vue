<template>
<h2> Nuevos Partidos:</h2>
  <div class="nuevoPartidos">
    <form>
      <label >Numero de la Jornada:</label><br>
      <input type="text"  v-model="numerojornada"><br>
      <label >Fecha del Encuentro:</label><br>
      <input type="date" v-model="fechaJornada"><br>
      <label >Equipo Local:</label><br>
      <select name="select" v-model="equipo1">
        <option >...</option>
        <option  v-for="equipo in equipos" :key="equipo">{{equipo.team1}}</option>
      </select><br>
      <label >Equipo Visitante:</label><br>
      <select name="select" v-model="equipo2">
        <option value="">...</option>
        <option v-for="equipo in equipos" :key="equipo">{{equipo.team2}}</option>
      </select><br>
      <button type="button"  @click="crearJornada()">enviar</button>
    </form>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
        return {
            equipos: [],
            numerojornada:"",
            fechaJornada:"",
            equipo1:"",
            equipo2:""
        }
    },
  
   mounted() {
        axios
        .get("http://localhost:3000/matches")
        .then(response => {
          this.equipos = response.data;
        })
        .catch(response=>alert("Error al recuperar datos "+response.status))
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

<style>

.nuevoPartidos{
  width: 100%;
}

</style>
