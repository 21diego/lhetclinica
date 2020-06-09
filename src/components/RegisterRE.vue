<template>
  <div>
    <div class="container">
      <h1>Registro Enfermería</h1>

      <form id="formulario" action>
        <div id="overflow">
          <!-- Usamos una clase "tab" para cada pestaña del formulario (son 2) -->
          <!------------- TAB 1 (Datos) ------------>
          <div class="tab">
            <h4>Información regular</h4>
            <div class="form-group d-flex flex-nowrap">
              <label for="id" class="d-flex align-items-center justify-content-center">DNI:</label>
              <input name="id" type="number" class="requiredInput form-control" placeholder="DNI" />
            </div>

            <div class="form-group d-flex flex-nowrap">
              <label for="fechaHora" class="d-flex align-items-center justify-content-center">Fecha y hora:</label>
              <input name="fechaHora" type="date" class="requiredInput form-control" placeholder="fechaHora"/>
            </div>
          </div>


            <!------------- TAB 2 (Examenes) ------------>
          <div class="tab">
            <div class="form-group d-flex flex-nowrap">
              <label for="tensionArterial" class="d-flex align-items-center justify-content-center">Tensión arterial:</label>
              <inpup name="tensionArterial" type="number" class="requiredInput form-control" placeholder="Tenseión arterial"/>
            </div>

            <div class="form-group d-flex flex-nowrap">
              <label for="frecuenciaCardiaca" class="d-flex align-items-center justify-content-center">Frecuencia cardiaca:</label>
              <input name="frecuenciaCardiaca" type="number" class="requiredInput form-control" placeholder="Frecuencia cardiaca"/>
            </div>

            <div class="form-group d-flex flex-nowrap">
              <label for="frecuenciaRespiratoria" class="d-flex align-items-center justify-content-center">Frecuencia respiratoria:</label>
              <input name="frecuenciaRespiratoria" type="number" class="requiredInput form-control" placeholder="Frecuencia respiratoria"/>
            </div>

            <div class="form-group d-flex flex-nowrap">
              <label for="temperatura" class="d-flex align-items-center justify-content-center">Temperatura:</label>
              <input name="temperatura" type="number" class="requiredInput form-control" placeholder="Temperatura"/>
            </div>
          </div>


            <!------------- TAB 2 (Observaciones) ------------>
          <div class="tab">
            <div class="form-group d-flex flex-nowrap">
              <label for="observaciones" class="d-flex align-items-center justify-content-center">Observaciones</label>
              <textarea name="observaciones" class="requiredInput form-control" placeholder="Observaciones"
              ></textarea>
            </div>
          </div>

        </div>

        <!---------- BOTONES DE CAMBIO DE PESTAÑA ------------->
      <div style="overflow:scroll;">
        <div id="stepNav">
          <button type="button" id="prevBtn" class="btn btn-primary mr-1" v-on:click="nextPrev(-1)" >Atras</button>
          
          <!---------- PUNTOS QUE INDICAN EL PROGRESO DEL FORM  ------------>
          <div id="steps">
            <span class="step"></span>
            <span class="step"></span>
            <span class="step"></span>
          </div>
          
          <button type="button" id="nextBtn" class="btn btn-primary" v-on:click="nextPrev(1)" >Siguiente</button>
          <button id="submit" class="submit btn btn-primary" v-on:click="updateForm">Enviar</button>
        </div>
      </div>


      </form>
    </div>
  </div>
</template>


<script>
export default {
    name:"RegisterRE",

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

    postRE() {
        fetch("/api/pacientes/" + this.$route.params.id + "/re", {
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

            fetch("/api/pacientes/" + this.$route.params.id + "/re", {
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
    }}
}
</script>