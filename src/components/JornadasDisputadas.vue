<template>
  <div class="jugadoresDiv">
        <span v-for="jornada in jornadas" :key="jornada" >
          <p v-if="this.numeroJornada==jornada.round && jornada.score !=undefined" >
            {{jornada.team1}}{{jornada.score[0]}}-{{jornada.score[1]}}{{jornada.team2}}
          </p>
          <p v-if="this.numeroJornada==jornada.round && jornada.score ==undefined" >
            {{jornada.team1}}<input type="text">---<input type="text">{{jornada.team2}} <input type="submit" value="guardar">
          </p>
        </span>
  </div>
</template>

<script>
import axios from "axios";


export default {
  props:['numeroJornada'],

    data() {
        return {
            jornadas: []
        }
    },
    mounted() {
      axios
        .get("http://localhost:3000/matches/")
        .then(response => {
          this.jornadas = response.data;
        })
        .catch(response=>alert("Error al recuperar datos "+response.status));;
    }
}
</script>

<style >
input{
  width: 10%;
}
.jugadoresDiv{
  width: 90%;
  margin-top: 100px;
  margin-left: 300px;
}
</style>