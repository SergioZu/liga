<template>
  <h2> La Clasificacion de la liga:</h2>
  <div class="clasificacion">
    <table style="width:100%">
      <tr>
        <th>Nombre</th>
        <th>Puntos</th> 
      </tr>
      <tr v-for="articulo in liga" :key="articulo.id">
        <td ><router-link to="/">{{ articulo.name }}</router-link></td>
        <td >{{ articulo.points }}</td>
      </tr>
    </table>
  </div>
  
</template>

<script>
import axios from "axios";

export default {
    data() {
        return {
            liga: []
        }
    },
    mounted() {
      axios
        .get("http://localhost:3000/clubs")
        .then(response => {
          this.liga = response.data;
        })
        .catch(response=>alert("Error al recuperar datos "+response.status));;
    }
}
</script>

<style>

.clasificacion{
  width: 30%;
  margin-left: 20%;
 
}
table, th, td {
  border: 1px solid black;
  border-collapse: collapse;
}
th, td {
  padding: 15px;
}

</style>