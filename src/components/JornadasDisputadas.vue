<template>
  <div class="jugadoresDiv">
        <span v-for="jornada in jornadas" :key="jornada" >
          <p v-if="this.numeroJornada==jornada.round && jornada.score !=undefined" >
            {{jornada.team1}} {{jornada.score[0]}} - {{jornada.score[1]}} {{jornada.team2}}
          </p>
          <p v-if="this.numeroJornada==jornada.round && jornada.score ==undefined" >
            {{jornada.team1}} <input type="number" v-model="resultadoLocal"> --- <input type="number" v-model="resultadoVisitante"> 
            {{jornada.team2}} <button type="button"  @click="actualizarJornada(jornada)">enviar</button>
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
            jornadas: [],
            equipos:[],
            resultadoLocal:"",
            resultadoVisitante:""
        }
    },
    mounted() {
      axios
        .get("http://localhost:3000/matches/")
        .then(response => {
          this.jornadas = response.data;
        })
        .catch(response=>alert("Error al recuperar datos "+response.status));;
    },
    methods:{
      actualizarJornada: function(jornada){
        let matches={
          round:jornada.round,
          date:jornada.date,
          team1:jornada.team1,
          team2:jornada.team2,
          score:[
            this.resultadoLocal,
            this.resultadoVisitante
          ]
        }
        this.añadirPuntos(matches); 
        // axios.put("http://localhost:3000/matches/"+jornada.id, matches).then((result) => {
        //     alert("Se ha actualizado la Jornada Correctamente");
        //     });
       
           
      
      },añadirPuntos: function(jornadas){
      console.log(jornadas.team1+" "+equipos)

       axios
        .get("http://localhost:3000/clubs/")
        .then(response => {
          this.equipos = response.data; 
          for (let index = 0; index < this.equipos.length; index++) {
            if(jornadas.team1==this.equipos.name){
              if(jornadas.score[0]>jornadas.score[1]){

              }
              if(jornadas.score[0]<jornadas.score[1]){

              }
              if(jornadas.score[0]==jornadas.score[1]){

              }
            }
              
          }
        
        })
        .catch(response=>alert("Error al recuperar datos "+response.status));
      
    }
      
    }
}
</script>

<style >
input{
  width: 20%;
}
.jugadoresDiv{
  width: 170%;
margin-top: 100px;
margin-left: 135px;
}
</style>