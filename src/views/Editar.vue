<template>
  <div class="formulario">

    <h1> Editar recorrido </h1>
        <form @submit.prevent="guardar">

            <v-text-field
                v-model="objeto.nombre"
                label="Nombre"
                type="text"
            ></v-text-field>
          <v-text-field
              v-model="objeto.distancia"
              label="Distancia en km"
              type="number"
              min="1"
          ></v-text-field>
          <v-text-field
              v-model="objeto.duracion"
              label="Duración en minutos"
              type="number"
              min="1"
          ></v-text-field>

            <v-select
                v-model="objeto.ambito"
                :items="select"
                label="Ámbito"
                data-vv-name="objeto.ambito"
                required

            ></v-select>
          <v-text-field
              v-model="objeto.dia"
              label="Día"
              type="date"
          ></v-text-field>

          <v-btn
              class="ml-auto mt-4"
              type="submit"
              color="primary">
            Actualizar
          </v-btn>
        </form>

    <div v-if="envio === true">
      <div v-if="cantErrores" class="errores">
        <ul>
          <li v-for="error in errores" :key="error"> {{error}} </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Ingresar",

  data: function () {
    return {
      objeto:{
        nombre:this.$route.params.nombre,
        duracion : this.$route.params.duracion,
        distancia : this.$route.params.distancia,
        ambito: this.$route.params.ambito,
        dia:this.$route.params.dia,
        fecha:this.$route.params.fecha
      },
      select: [{ text: 'Selecione uno', value: null }, 'Ciudad', 'Montaña'],
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
      if (!this.objeto.nombre) {
        this.errores.push('El nombre del recorrido es obligatorio')
      }
      if (!this.objeto.distancia) {
        this.errores.push('La distancia es obligatoria')
      }
      if (!this.objeto.distancia > 1) {
        this.errores.push('La distancia debe ser mayor a 0')
      }
      if (!this.objeto.duracion) {
        this.errores.push('La duración es obligatoria')
      }
      if (!this.objeto.duracion > 1) {
        this.errores.push('La duración debe ser mayor a 0')
      }
      if (this.objeto.ambito == null) {
        this.errores.push('Debe ingresar un ámbito')
      }
      if (this.objeto.dia == null) {
        this.errores.push('Debe ingresar una fecha válida')
      }

      if (this.errores.length == 0) {

        let nuevoObjeto = {
          nombre: this.objeto.nombre,
          distancia: this.objeto.distancia,
          duracion: this.objeto.duracion,
          ambito: this.objeto.ambito,
          dia: this.objeto.dia,
          fecha:this.objeto.fecha

        }
        console.log(nuevoObjeto.nombre);
        this.storage = JSON.parse(localStorage.getItem("data"))
        for (let i = 0; i < this.storage.length; i++) {
          if (this.storage[i].fecha == nuevoObjeto.fecha) {
            this.storage.splice(i, 1);
          }
        }
        this.storage.push(nuevoObjeto);
        localStorage.setItem("data", JSON.stringify(this.storage))
        this.$router.push('/recorrido');
      }
    },

    },


}


</script>

<style>
.formulario {
  width: 80%;
  margin: 2em auto;
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


.envioOk p {
  margin: .5em auto;
}

</style>