<template>
  <div id="register">
    <BackBtn />
    <form id="formulario" v-on:submit.prevent="register">
      <h2 id="title">Complete sus datos:</h2>
      <div class="form-group d-flex flex-nowrap">
        <label for="nombre" class="d-flex align-items-center justify-content-start">
          <i class="fas fa-user"></i>
          <div class="name-input">Nombre: </div>
        </label>
        <input id="nombre" type="text" class="form-control" name="nombre" value="" required autofocus placeholder="Nombre"/>
      </div>
      <div class="form-group d-flex flex-nowrap">
        <label for="apellido" class="d-flex align-items-center justify-content-start">
          <i class="fas fa-user"></i>
          <div class="name-input">Apellido: </div>
        </label>
        <input id="apellido" type="text" class="form-control" name="apellido" value="" required autofocus placeholder="Apellido"/>
      </div>
      <div class="form-group d-flex flex-nowrap">
        <label for="documento" class="d-flex align-items-center justify-content-start">
          <i class="fas fa-address-card"></i>
          <div class="name-input">Documento: </div>
        </label>
        <input id="documento" type="text" class="form-control" name="documento" value="" required autofocus placeholder="Documento"/>
      </div>
      <div class="form-group d-flex flex-nowrap">
        <label for="telefono" class="d-flex align-items-center justify-content-start">
          <i class="fas fa-phone"></i>
          <div class="name-input">Telefono: </div>
        </label>
        <input id="telefono" type="number" class="form-control" name="telefono" value="" required autofocus placeholder="Telefono"/>
      </div>
      <div class="separador"></div>
      <div id="pregunta" class="d-flex">
        <label for="doctor" class="d-flex align-items-center justify-content-start">¿Sos medico?</label>
        <input id="doctor" type="checkbox" value="" v-on:change="ableDoctorInput" class="form-control" name="doctor" autofocus/>
      </div>
      <div class="form-group d-flex flex-nowrap">
        <label for="matricula" class="d-flex align-items-center justify-content-start">
          <i id="icon-matricula" class="fas fa-id-card-alt disable"></i>
          <div class="name-input">Matricula: </div>
        </label>
        <input id="matricula" type="text" class="form-control disable" name="matricula" value="" autofocus placeholder="Matricula" disabled/>
      </div>
      <div class="separador"></div>
      <div class="form-group d-flex flex-nowrap">
        <label for="email" class="d-flex align-items-center justify-content-start">
          <i class="fas fa-envelope"></i>
          <div class="name-input">Email: </div>
        </label>
        <input id="email" type="email" class="form-control" name="email" value="" required autofocus placeholder="Email"/>
      </div>
      <div class="form-group d-flex flex-nowrap">
        <label for="password" class="d-flex align-items-center justify-content-start">
          <i class="fas fa-lock"></i>
          <div class="name-input">Contraseña: </div>
        </label>
        <input id="password" type="password" class="form-control" name="password" value="" required autofocus placeholder="Contraseña"/>
      </div>
      <button type="submit" class="btn-reg">Registrarse</button>
    </form>
  </div>
</template>


<script>
import { mapMutations } from 'vuex';
import BackBtn from '@/components/BackBtn.vue';

export default {
  name: 'Register',
  components: { BackBtn },
  data() {
    return {
      form: { nombre: "",
        apellido: "",
        matricula: "",
        telefono: "",
        email: "",
        password: ""
      },
      error: null
    };
  },
  methods: {
    ...mapMutations(['st_logUser']),
    register(){
      window.localStorage.setItem('user','true');
        this.st_logUser();
        this.$router.push('/').catch(err => {});
    },
    ableDoctorInput(){
      let check = document.querySelector('#doctor').checked;
      document.querySelector('#matricula').disabled = !check;
      document.querySelector('#matricula').value = "";
      document.querySelector('#matricula').classList.toggle('disable');
      document.querySelector('#icon-matricula').classList.toggle('disable');
    }
  }

}
</script>
<style scoped>
#register{
  height: 100vh;
  background: white;
}
#formulario{
  padding: 0 2em;
  padding-top: 75px;
}
.name-input{
  display: none;
}
#formulario label{
  margin: 0;
}
#formulario .fas{
  color: white;
  background-color: rgb(13, 123, 202);
  font-size: 1.2em;
  border: 1px rgb(13, 123, 202) solid;
  padding: 0.3em;
  border-radius: 50%;
  width: 35px;
  height: 35px;
  margin-right: 0.4em;
}
#formulario .fas.disable{
  background-color: grey;
  border: 1px grey solid;
}
#pregunta{
  margin: 0 auto;
  margin-bottom: 1em;
  width: 50%;
}
#doctor{
  font-size: 0.3em!important;
  width: 50px!important;
}
#formulario input{
  background: none;
  border: none;
  border-bottom: 2px solid rgb(13, 123, 202);
  color: rgb(13, 123, 202);
  font-size: 1em;
  font-weight: bold;
}
#formulario input::placeholder{
  color: rgba(16, 123, 199, 0.7);
}
#formulario input.disable{
  border-bottom: 2px solid grey;
}
#formulario input.disable::placeholder{
  color: grey!important;
}
#formulario input:focus{
  outline:none !important;
  outline-width: 0 !important;
  box-shadow: none;
}
#title{
  font-size: 2em;
  margin-bottom: 1em;
  color: rgb(13, 123, 202);
}
#formulario .separador{
  width: 100%;
  margin: 1em 0;
  height: 0px;
  border-bottom: 1px solid rgb(13, 123, 202,0.3);
}
.btn-reg{
  border-radius: 1em;
  background: rgb(13, 123, 202);
  border: 2px rgb(13, 123, 202) solid;
  cursor: pointer;
  color: white;
  width: 100%;
  font-size: 1.2em;
  font-weight: bold;
  padding: 0.2em 0;
  margin: 0.5em 0;
}
@media screen and (min-width: 1000px){
  #register{
    border-radius: 0.5em;
  }
  #formulario{
    width: 550px;
    margin: 0 auto;
  }
  .name-input{
    display: inline;
    margin: 0 1em;
    font-size: 1.3em;
  }
  #formulario input::placeholder{
    color: white;
  }
  #formulario input.disable::placeholder{
    color: white!important;
  }
  .form-group{
    justify-content: space-between;
  }
  .form-group>label{
    width: 35%;
  }
  .form-group>input{
    width: 60%;
  }

}

</style>
