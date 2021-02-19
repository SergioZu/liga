<template>
  <h2> Jornadas de la liga:</h2>
  <div class="clasificacion">
   <select name="select" >
    <option value="">...</option>
    <option v-for="jornada in jornadas" :key="jornada" @click="obtenerJornada(jornada.round)">{{jornada.round}}</option>
   </select>    
  </div>

   <div class="listadoJornadas" v-if="controlJornada!=''">
    <JornadasDisputadas v-bind:numeroJornada="controlJornada"/>
  </div>
</template>

<script>
import axios from "axios";
import JornadasDisputadas from '@/components/JornadasDisputadas.vue'
export default {
    data() {
        return {
            jornadas: [],
            controlJornada:""
        }
    },
  
  methods:{
    generarArray: function(){
        axios
        .get("http://localhost:3000/matches")
        .then(response => {
          this.jornadas = response.data;
        })
        .catch(response=>alert("Error al recuperar datos "+response.status))
    },
    obtenerJornada: function(jornadaActual){
      this.controlJornada=jornadaActual;
    }

  },
   created(){
       this.generarArray();
    },
    components: {
    JornadasDisputadas
  }
}
</script>

<style scoped>

.clasificacion{
  width: 30%;
  margin-left: 35%;
  position: absolute;
 
}

.listadoJornadas{
   width: 30%;
  margin-left: 20%;
  position: absolute;
}

</style>