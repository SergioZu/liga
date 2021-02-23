<template>
  <h2> Eliminar Jugadores:</h2>
  <div class="clasificacion">
      <p>Equipo: </p>
    <select name="select" v-model="equipoActual" @click="actualizarSelect()">
        <option >...</option>
        <option  v-for="equipo in equipos" :key="equipo">{{equipo.name}}</option>
      </select><br>
      <label >Jugadores:</label><br>
      <select name="select">
        <option value="">...</option>
        <option v-for="jugador in select" :key="jugador"  @click="activarBoton(jugador)">
            {{jugador.name}}
        </option>
      </select><br>
      <div v-if="activado==false">
         <button type="button" disabled>enviar</button> 
      </div>
      <div v-if="activado!=false">
         <button type="button"  @click="EliminarJugador()">enviar</button> 
      </div>
      
  </div>
 
</template>

<script>
import axios from "axios";
import FormularioJugador from '@/components/FormularioJugador.vue'
export default {
    data() {
        return {
            equipos: [],
            jugadores:[],
            select:[],
            equipoActual:"",
            idjugador:"",
            activado:false
        }
    },
    created() {
      axios
        .get("http://localhost:3000/clubs")
        .then(response => {
          this.equipos = response.data;
        })
        .catch(response=>alert("Error al recuperar datos "+response.status));

        axios
        .get("http://localhost:3000/players")
        .then(response => {
          this.jugadores = response.data;
        })
        .catch(response=>alert("Error al recuperar datos "+response.status));
    },
  methods:{
    EliminarJugador: function(){
      axios.delete("http://localhost:3000/players/"+this.idjugador).then((result) => {
          
          alert("Se ha eliminado el Jugador")
        }); 
    },actualizarSelect:function(){
        this.select=[];
        for (let index = 0; index < this.jugadores.length; index++) {
            if (this.jugadores[index].team==this.equipoActual) {
                this.select.push(this.jugadores[index]);
            }
            
        }
    },activarBoton:function(jugador){
        this.idjugador=jugador.id;
        if(this.equipoActual!=''){
            this.activado=true;
        }
    }
  },
    components: {
    FormularioJugador
  },
}
</script>

<style scoped>

.clasificacion{
  width: 30%;
  margin-left: 35%;
  position: absolute;
 
}


</style>