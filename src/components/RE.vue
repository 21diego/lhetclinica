<template>
  <div>
    <ul>
      <li class="cardd" v-for="(registro,index) in registros" :key="index">
        <router-link to="/detalleRE">
          <div @click="st_cargarREActual(registro)">{{registro.fechaHora}}: {{registro.observaciones}}</div>
        </router-link>
        <i class="fas fa-caret-right"></i>
      </li>
    </ul>
    <router-link :to="'/registerre/'+paciente.id" class="btn btn-large">+Nuevo Registro</router-link>
  </div>
</template>

<script>
// @ is an alias to /src
import { mapMutations, mapState } from 'vuex';

export default {
  name: 'FM',
  props: ['paciente'],
  data() {
    return {
      registros: []
    };
  },
  created(){
    this.cargarActual()
  },
  methods: {
    ...mapMutations(['st_cargarREActual']),
    async cargarActual(){
      await fetch(`/api/pacientes/${this.paciente.id}/registros`)
      .then(response => {
        if(response.ok){
          return response.json()
        }else{
          return Promise.reject(res)
        }
      }).then(json => this.registros= json.registros.sort(this.sortByDate))
      .catch(error => {
        console.log(error)
      })
    },
    sortByDate(a, b){
      return Date.parse(b.registro.ingreso) - Date.parse(a.registro.ingreso);
    }
  },
}
</script>
<style scoped>
      .cardd{
        border:1px solid #007bff;
        border-radius: 0.3em;
        margin: 0.3em 0;
        display: flex;
        justify-content: space-between;
        padding: 1em;
      }
      ul{
        width: 90%;
        margin: 0 auto;
        padding: 0;
      }
      li{
        cursor: pointer;
        list-style: none;
        text-transform: capitalize;
      }
      li i{
        color: #007bff;
        font-size: 1.5em;
      }
</style>