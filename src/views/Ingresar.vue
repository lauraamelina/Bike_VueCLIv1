<template>
  <div class="formulario">

    <h1> Ingresar recorridos </h1>
        <form @submit.prevent="guardar">

            <v-text-field
                v-model="nombre"
                label="Nombre"
                type="text"
            ></v-text-field>
          <v-text-field
              v-model="distancia"
              label="Distancia en km"
              type="number"
              min="1"
          ></v-text-field>
          <v-text-field
              v-model="duracion"
              label="Duración en minutos"
              type="number"
              min="1"
          ></v-text-field>

            <v-select
                v-model="ambito"
                :items="select"
                label="Ámbito"
                data-vv-name="ambito"
                required

            ></v-select>
          <v-text-field
              v-model="dia"
              label="Día"
              type="date"
          ></v-text-field>

          <v-btn
              class="ml-auto mt-4"
              type="submit"
              color="primary"
          >
            Ingresar
          </v-btn>

        </form>





    <div v-if="envio === true">
      <div v-if="cantErrores" class="errores">
        <ul>
          <li v-for="error in errores" :key="error"> {{error}} </li>
        </ul>
      </div>

      <div v-else class="envioOk">

        <p> Recorrido registrado </p>

      </div>


    </div>



  </div>
</template>

<script>
export default {
  name: "Ingresar",

  data: function () {
    return {

      distancia: null,
      duracion: null,
      select: [{ text: 'Selecione uno', value: null }, 'Ciudad', 'Montaña'],
      ambito: null,
      nombre: "",
      dia: null,
      fecha:null,


      errores: [],
      envio: false,
      array: [],

    }
  },
  computed: {
    cantErrores: function () {
      return this.errores.length;

    }
  },

  methods: {
    guardar: function () {

      this.envio = true;
      this.errores = [];
      if (!this.nombre) {
        this.errores.push('El nombre del recorrido es obligatorio')
      }
      if (!this.distancia) {
        this.errores.push('La distancia es obligatoria')
      }
      if (!this.distancia > 1) {
        this.errores.push('La distancia debe ser mayor a 0')
      }
      if (!this.duracion) {
        this.errores.push('La duración es obligatoria')
      }
      if (!this.duracion > 1) {
        this.errores.push('La duración debe ser mayor a 0')
      }
      if (this.ambito == null) {
        this.errores.push('Debe ingresar un ámbito')
      }
      if (this.dia == null) {
        this.errores.push('Debe ingresar una fecha válida')
      }

      if (this.errores.length == 0) {

       let nuevoObjeto = {
          nombre: this.nombre,
          distancia: this.distancia,
          duracion: this.duracion,
          ambito: this.ambito,
          dia: this.dia,
         fecha: new Date().getTime()
        }


        if (!localStorage.data) {
          this.array = []
        } else {
          this.array = JSON.parse(localStorage.getItem("data"))
        }

        this.array.push(nuevoObjeto);
        localStorage.setItem("data", JSON.stringify(this.array));

        this.nombre = "";
        this.distancia = "";
        this.duracion = "";
        this.ambito = null;
        this.dia = null;

      }
    },


  }
}

</script>

<style>

.formulario {
  width: 80%;
  margin: 2em auto;
}

ul {
  padding-left: 0 !important;
}

.errores {
  color: white;
  width: 100%;
  margin: 1em auto;
  text-align: center;
}

.errores li {
  color: white;
  font-weight: bold;
  border: none;
  padding: 10px;
  margin: .5em auto;
  border-radius: 5px;
  background: #8d1414;
}

.errores li:hover {
  background-color: #e2221f8e;

}

.envioOk {
  border: solid 1px rgb(62, 190, 62);
  background: #18911e;
  width: 100%;
  color: white;
  font-weight: bold;
  text-align: center;
  margin: 1em auto;
  border-radius: 5px;
}

.envioOk p {
  margin: .5em auto;
}

</style>