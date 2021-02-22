<template>
  <h2> Jornadas de la liga:</h2>
  <div class="clasificacion">
   <select name="select">
    <option value="">...</option>
        <option v-for="jornada in jornadasAux" :key="jornada"  @click="obtenerJornada(jornada)">{{jornada}}</option>
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
            jornadasAux:[],
            controlJornada:"",
            aux:""
        }
    },
  
  methods:{
    generarArray: function(){
        axios
        .get("http://localhost:3000/matches")
        .then(response => {
          this.jornadas = response.data;


          for(var i = 0; i < this.jornadas.length; i++) {  
            if (this.aux!=this.jornadas[i].round) {
               this.aux = this.jornadas[i].round;
              this.jornadasAux.push( this.aux);
            }
          }


        })
        .catch(response=>alert("Error al recuperar datos "+response.status))
        
    },
    obtenerJornada: function(jornadaActual){
      this.controlJornada=jornadaActual;
    }

  },
   created(){
     this.generarArray()
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