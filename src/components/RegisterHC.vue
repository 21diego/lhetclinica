<template>
  <div>
    
  <div class="container">
      <h1>Registro Historia Clínica</h1>
      

      
    <form id="formulario" action>
      <div id="overflow">
        <!-- Usamos una clase "tab" para cada pestaña del formulario (son 3) -->

        <!------------- TAB 1 (Datos personales) ------------>
        <!--FALTA AGREGAR MEDICAMENTOS-->
        <div class="tab">
          <h4>Información básica:</h4>
          <div class="form-group d-flex flex-nowrap">
            <label for="motivo_consulta" class="d-flex align-items-center justify-content-center">
              Motivo de Consulta:
            </label>
            <input name="motivo_consulta" type="text" class="requiredInput form-control" placeholder="Motivo" />
          </div>

          <div class="form-group d-flex flex-nowrap">
            <label for="diagnostico" class="d-flex align-items-center justify-content-center">
              Diagnostico
            </label>
            <select name="diagnostico" id="">
              <option value="PCR_SOLICITADO">PCR Solicitado</option>
              <option value="PCR_NO_SOLICITADO">PCR no solicitado</option>
              <option value="PCR_POSITIVO">PCR positivo</option>
              <option value="PCR_NEGATIVO">PCR negativo</option>
              <option value="ALTA">Alta</option>
            </select>
          </div>

          <div class="form-group d-flex flex-nowrap">
            <label for="sintomas" class="d-flex align-items-center justify-content-center">
              Síntomas
            </label>
            <input name="sintomas" type="text" class="requiredInput form-control" placeholder="Síntomas"/>
          </div>

          <div class="form-group d-flex flex-nowrap">
            <label for="tratamiento" class="d-flex align-items-center justify-content-center">
              Tratamiento
            </label>
            <input name="tratamiento" type="text" class="requiredInput form-control" placeholder="Tratamiento" />
          </div>
        </div>

        

        <!------------- TAB 2 (Examenes 1/2) --------------->
        <div class="tab">
        
          <h4>Exámenes complementarios</h4>

          <div class="form-group d-flex flex-nowrap">
            <label for="temperatura" class="d-flex align-items-center justify-content-center">
              Temperatura
            </label>
            <input name="temperatura" type="number" class="requiredInput form-control" placeholder="Cantidad"/>
          </div>


          <div class="form-group d-flex flex-nowrap">
            <label for="eritrosedimentacion" class="d-flex align-items-center justify-content-center">
              Eritrosedimentación
            </label>
            <input name="eritrosedimentacion" class="requiredInput form-control" placeholder="Velocidad" />
          </div>

          <div class="form-group d-flex flex-nowrap">
            <label for="globulos_blancos" class="d-flex align-items-center justify-content-center">
              Glóbulos blancos
            </label>
            <input name="globulos_blancos" type="number" class="requiredInput form-control" placeholder="Cantidad" />
          </div>

          <div class="form-group d-flex flex-nowrap">
            <label for="globulos_rojos" class="d-flex align-items-center justify-content-center">
              Glóbulos rojos
            </label>
            <input name="globulos_rojos" type="number" class="requiredInput form-control" placeholder="Cantidad"/>
          </div>

          <div class="form-group d-flex flex-nowrap">
            <label for="plaquetas" class="d-flex align-items-center justify-content-center">
              Plaquetas
            </label>
            <input name="plaquetas" type="number" class="requiredInput form-control" placeholder="Cantidad"/>
          </div>
        </div>

        <!------------- TAB 3 (Examenes 2/2) --------------->
        <div class="tab">
          <div class="form-group d-flex flex-nowrap">
            <label for="observaciones" class="d-flex align-items-center justify-content-center">
              Observaciones
            </label>
            <textarea name="observaciones" class="requiredInput form-control" placeholder="Observaciones"></textarea>
          </div>
        </div>

      </div>


      <!---------- BOTONES DE CAMBIO DE PESTAÑA ------------->
      <div style="overflow:scroll;">
        <div id="stepNav">
          <button
            type="button"
            id="prevBtn"
            class="btn btn-primary mr-1"
            v-on:click="nextPrev(-1)"
          >Atras</button>
          <!---------- PUNTOS QUE INDICAN EL PROGRESO DEL FORM  ------------>
          <div id="steps">
            <span class="step"></span>
            <span class="step"></span>
            <span class="step"></span>
          </div>
          <button
            type="button"
            id="nextBtn"
            class="btn btn-primary"
            v-on:click="nextPrev(1)"
          >Siguiente</button>
          <button id="submit" class="submit btn btn-primary" v-on:click="updateForm">Enviar</button>
        </div>
      </div>

      <!---------- PUNTOS QUE INDICAN EL PROGRESO DEL FORM  ------------>
    </form>
      
      
    </div>
  </div>
</template>

<script>


export default {
  name: "RegisterHC",
  
  data() {
    return {
      currentTab: 0,
      recordIndex: 0,
    };
  },
  // Al montarse, se muestra la primera pestaña
  mounted: function() {
    this.showTab(this.currentTab);
    this.markRequired();
  },
  methods: {
    
    cancelSubmit(e) {
      e.preventDefault();
      this.adding = false;
    },
    showTab(n) {
      var allTabs = document.getElementsByClassName("tab");
      allTabs[n].style.display = "block";
      if (n == 0) {
        document.getElementById("prevBtn").style.opacity = "0";
      } else {
        document.getElementById("prevBtn").style.opacity = "1";
      }
      if (n == allTabs.length - 1) {
        document.getElementById("nextBtn").style.display = "none";
        document.getElementById("submit").style.display = "inline";
      } else {
        document.getElementById("nextBtn").style.display = "inline";
        document.getElementById("submit").style.display = "none";
      }
      // ... Muestra el paso correspondiente
      this.fixStepIndicator(n);
    },

    nextPrev(n) {
      // Se fija qué pestaña hay que mostrar
      var allTabs = document.getElementsByClassName("tab");
      if (n == 1 && !this.validateForm()) return false;
      // Oculta la pestaña actual
      allTabs[this.currentTab].style.display = "none";
      // Incrementa o disminuye la pestaña actual en 1 o en -1
      this.currentTab = this.currentTab + n;
      // Si llegas al final del form, se hace el submit
      if (this.currentTab >= allTabs.length) {
        document.getElementById("regForm").submit();
        return false;
      }
      //Si no, muestra la próxima pestaña
      this.showTab(this.currentTab);
    },

    validateForm() {
      var allTabs,
        allInputs,
        valid = true;
      allTabs = document.getElementsByClassName("tab");
      allInputs = allTabs[this.currentTab].getElementsByClassName(
        "requiredInput"
      );
      // Loop que checkea los inputs de la pestaña acuatl
      for (let i = 0; i < allInputs.length; i++) {
        if (allInputs[i].value == "") {
          allInputs[i].className += " invalid";
          valid = false;
        } else {
          allInputs[i].classList.remove("invalid");
        }
      }
      // Si value es true, agrega un paso a los puntos de abajo
      if (valid) {
        document.getElementsByClassName("step")[this.currentTab].className +=
          " finish";
        return true;
      } else {
        alert("Faltan campos por rellenar.");
        return false;
      }
    },
    fixStepIndicator(n) {
      var allSteps = document.getElementsByClassName("step");
      for (let i = 0; i < allSteps.length; i++) {
        allSteps[i].className = allSteps[i].className.replace(" active", "");
      }
      allSteps[n].className += " active";
    },

postHC() {
      fetch("/api/pacientes/" + this.$route.params.id + "/hc", {
        method: "POST",
        headers: {
          "Content-Type": "application/json"
        },
        body: JSON.stringify()
      });
    },

    updateForm(e) {
      e.preventDefault();
      let self = this;
      if (this.validateForm()) {
        let formElem = document.getElementById("formulario");
        let formData = new FormData(formElem);

        let object = {};
        formData.forEach((value, key) => {
            object[key] = value;
          });
        let json = JSON.stringify(object);

        fetch("/api/pacientes/" + this.$route.params.id + "/hc", {
          method: "POST",
          headers: {
            "Content-Type": "application/json"
          },
          body: json
        })
          .then(res => {
            if (res.ok) {
              return res.json();
            } else {
              return new Promise.reject(res.json());
            }
          })

          .then(jsonPaciente => {
            fetch("/api/all/pacientes")
              .then(response => {
                if (response.ok) {
                  return response.json();
                } else {
                  return Promise.reject(response);
                }
              })
              .then(json => {
                this.st_cargarAllPacientes(json.pacientes);
                this.$router
                  .push(`/paciente/${jsonPaciente.pacienteId}`)
                  .catch(err => {});
              })

              .catch(error => {
                console.log(error);
              });
          });
      } else {
        return;
      }
    },

    markRequired() {
      let inputs = document.getElementsByClassName("requiredInput");
      inputs.forEach(input => {
        if (input.type == "radio") {
          input.parentNode.previousSibling.previousSibling &&
          input.parentNode.previousSibling.previousSibling.childNodes[0]
            ? (input.parentNode.previousSibling.previousSibling.innerHTML +=
                '<span class="markedRequired">* </span>')
            : null;
        } else {
          input.previousSibling.innerHTML +=
            '<span class="markedRequired">* </span>';
        }
      });
    }

  },

};
</script>

<style scoped>
#dni-input {
  width: 40vw;
}
img {
  height: 5vh;
}
.lowOpacity {
  opacity: 0.4;
}
#diagnosisDiv,
#physicalExamsDiv,
#observationsDiv {
  width: 100%;
  min-height: 10vh;
  margin: 2vh 0vw;
  background-color: #ffffff;
  border: 1px solid black;
  border-radius: 0.5em;
}
#divsContainer {
  overflow: auto;
  height: 60vh;
}
#recordsNav {
  position: fixed;
  bottom: 1em;
  left: 0;
  right: 0;
}
.wrapper {
  overflow: auto;
  height: 65vh;
}
.w-95 {
  width: 95%;
}
input {
  border: 1px solid black;
  height: 16vh;
  width: 70vw;
}
label {
  font-size: x-large;
}
textarea {
  font-size: medium;
}

::-webkit-scrollbar {
  -webkit-appearance: none;
}

::-webkit-scrollbar:vertical {
  width: 6px;
}

::-webkit-scrollbar:horizontal {
  height: 6px;
}

::-webkit-scrollbar-thumb {
  background-color: rgba(0, 0, 0, 0.5);
  border-radius: 10px;
  border: 1px solid #ffffff;
}

::-webkit-scrollbar-track {
  border-radius: 6px;
  background-color: #ffffff;
}
#formulario {
  margin: 0 auto;
  padding: 0 2em;
}
#overflow {
  height: 60vh;
  overflow-y: scroll;
  margin-bottom: 1.5em;
  width: 100%;
  padding: 0 0.5em;
}
#formulario .fas {
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
#formulario .fas.disable {
  background-color: grey;
  border: 1px grey solid;
}
#formulario input {
  background: none;
  border: none;
  border-bottom: 2px solid rgb(13, 123, 202);
  color: rgb(13, 123, 202);
  text-align: left;
  font-size: 1em;
  font-weight: bold;
}
#formulario input::placeholder {
  color: rgba(16, 123, 199, 0.7);
}
#formulario input:focus {
  outline: none !important;
  outline-width: 0 !important;
  box-shadow: none;
}
#formulario select:focus {
  outline: none !important;
  outline-width: 0 !important;
  box-shadow: none;
}
.w-5em {
  width: 5em;
}

.markedRequired {
  font-weight: bold;
  color: red;
}
#formulario input.disable {
  border-bottom: 2px solid grey;
  color: grey;
}
#formulario input.disable::placeholder {
  color: grey !important;
}
input.invalid {
  color: #f03333cb !important;
  background-color: #ffdddd !important;
  border-bottom: 2px solid #fd4d4d !important;
}
#formulario input.invalid::placeholder {
  color: #f03333cb !important;
}
#formulario select.invalid {
  background: #ffdddd !important;
}

#preexistantConditions label {
  display: flex;
  justify-content: flex-start;
  text-align: start;
  align-self: center;
  font-size: 70%;

  margin-top: 2rem;
  font-size: larger;
}

#preexistantConditions label input {
  width: auto;
  align-self: center;
  margin: 0;
  margin-right: 0.3rem;
}
.tab {
  display: none;
}
button {
  width: 30%;
  padding: 0.5em 0;
  margin: 0;
}
#stepNav {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
#steps {
  width: 40%;
}
.step {
  height: 15px;
  width: 15px;
  margin: 0 2px;
  background-color: #007bff;
  border: none;
  border-radius: 50%;
  display: inline-block;
  opacity: 0.3;
}
.step.active {
  opacity: 0.7;
  border: rgba(44, 62, 80, 0.85) solid 3px;
}
.step.finish {
  background-color: #007bff;
  opacity: 1;
}

</style>
