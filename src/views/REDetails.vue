<template>
    <div>
        <Navbar/>
        <BackBtn />
        <div class="miPerfil">Perfil del paciente</div>
        {{this.st_REActual.fechaHora}}
        {{this.st_REActual.tensionArterial}}
        {{this.st_REActual.frecuenciaCardiaca}}
        {{this.st_REActual.frecuenciaRespiratoria}}
        {{this.st_REActual.temperatura}}
        {{this.st_REActual.observaciones}}
    </div>
</template>

<script>
import {mapState} from 'vuex';
import Navbar from '@/components/Navbar.vue';
import BackBtn from '@/components/BackBtn.vue';   
export default {
    components: { Navbar, BackBtn },

    computed: {
  ...mapState(['st_REActual']),
    },
    created(){
        console.log(st_allPacientes)
    }
}
</script>

<style scoped>
    .miPerfil{
    background-color: #007bff;
    color:white;
    font-size: 4vh;
    display: flex;
    justify-content: center;
    align-items: center;
    height: 70%;
    width: 100%;
  }
</style>