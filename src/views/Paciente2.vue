<template>
  <div>
    <Navbar />
    <div class="container">
      <div id="header">
      <h2 id="nameTitle" class="text-center">{{`${current.apellido}, ${current.nombre}`}}</h2>
      <div class="row m-0 p-0">
        <div id="history" v-on:click="change('history')" class="col-6 text active">Historia Clinica</div>
        <div id="nurse" v-on:click="change('nurse')" class="col-6 text">Registro de Enfermeria</div>
      </div> 
    </div>
    <div id="state">Estado: <span :class="current.estado">{{current.estado.diagnostico}}</span></div>
    <History v-if="currtentPage == 'history'" :anam="current.anamnesis" :contacts="current.contactosEmergencia"/>
    <Nurse v-if="currtentPage == 'nurse'" :registro="historia.registro"/>
    </div>
    
  </div>
</template>

<script>
import { mapState } from 'vuex'
import History from '@/components/History.vue'
import Nurse from '@/components/Nurse.vue'
import Navbar from '@/components/Navbar.vue'

  export default {
    name: 'Paciente',
    components: {
      History, Nurse, Navbar
    },
    data(){
      return {
        current: null,
        currtentPage: 'history',
        historia: null
      }
    },
    computed: {
      ...mapState(['st_misPacientes']),
      
    },
    methods: {
      cargarActual(){
        this.st_misPacientes.forEach(p => {
          if(p.id == this.$route.params.id){
            this.current = p;
          }
        });
      },
      change(text){
        this.currtentPage = text;
        let links = $('.text')
        for(let i= 0; i<links.length; i++){
          if(links[i].id===text){links[i].classList.add('active')}
          else{links[i].classList.remove('active')}
        }
      },
      async cargarRegistro(){
        await fetch('https://raw.githubusercontent.com/21diego/database/master/history.json')
        .then(response => response.json())
        .then(json => {
          json.forEach(r => {
            if(r.id == this.$route.params.id){
              this.historia = r;
            }
          })
        })
        .catch(error => console.log(error))
      }
    },
    created(){
      this.cargarRegistro();
      this.cargarActual();
    }
  }
</script>

<style scoped>
  #header{
    background-color: rgb(57, 158, 252);
    margin-top: 1em;
    border-radius: 0.3em;
  }
  #nameTitle{
    font-size: 2.3em;
    padding-top: 0.5em;
  }
  .text{
    font-size: 1.5em;
    padding: 0 1em;
  }
  .row div:hover, .active {
    background-color: rgba(51, 185, 141, 0.986);
  }
  #history{
    border-bottom-left-radius: 0.3em;
  }
  #nurse{
    border-bottom-right-radius: 0.3em;
  }
  #state{
    font-size: 1.5em;
  }
  .baja{
    color: green;
  }
  .alta{
    color: rgb(255, 208, 0);
  }
  .confirmado{
    color: red;
  }
</style>