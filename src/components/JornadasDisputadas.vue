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
        axios.put("http://localhost:3000/matches/"+jornada.id, matches).then((result) => {
            alert("Se ha actualizado la Jornada Correctamente");
               axios
          .get("http://localhost:3000/clubs")
          .then(response => {
            this.equipos = response.data;
            
             for (let index = 0; index < this.equipos.length; index++) {
               if(this.equipos[index].name==matches.team1){
                 if(matches.score[0]>matches.score[1]){
                   this.equipos[index].points+=3
                    console.log( this.equipos[index].points);
                 }else if(matches.score[0]==matches.score[1]){
                   this.equipos[index].points+=1
                 }
                 
               }else if(this.equipos[index].name==matches.team2){
                 if(matches.score[0]<matches.score[1]){
                   this.equipos[index].points+=3
                 }else if(matches.score[0]==matches.score[1]){
                   this.equipos[index].points+=1
                 }
                 
               }
          let equipo={
                    name:this.equipos[index].name,
                    id:this.equipos[index].id,
                    country:this.equipos[index].country,
                    points:this.equipos[index].points
                  }
                  axios.put("http://localhost:3000/clubs/"+equipo.id, equipo).then((result) => {
                       });

                  }
          })
          .catch(response=>alert("Error al recuperar datos "+response.status));
            });
       
       
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