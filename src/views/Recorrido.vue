<template>
  <div class="contenedor">
    <h1> Mis recorridos </h1>
    <v-container v-if="this.array.length > 0">
      <v-row>
        <v-col cols="12" md="6" lg="4" v-for="(x,index) in array" :key="index"
               >
          <v-card
              max-width="344"
              width="344"
              :class="[x.ambito=='Ciudad'?'ciudad':'montania']"
          >
            <v-card-text>
              <div>{{formatDate(x.dia)}}</div>
              <p class="text-h4 text--primary">
                {{x.nombre}}
              </p>
              <p>{{x.ambito}}</p>


              <div class="text--primary">
                {{x.distancia}} km<br>
                {{x.duracion}} minutos
              </div>
            </v-card-text>
            <v-card-actions class="px-3 pb-3">
              <v-flex text-xs-left>
                <v-btn
                    text
                    color="error"
                    @click="borrar(index)">
                  Borrar
                </v-btn>
              </v-flex>
              <v-flex text-xs-right>
                <v-btn
                    text
                   color="primary">
                  <v-icon left>
                    mdi-pencil
                  </v-icon>
                  <router-link :to="'/editar/' + x.nombre + '/' + x.distancia + '/' + x.duracion + '/' + x.ambito + '/' + x.dia + '/' + x.fecha"  class="editar"> Editar </router-link>

                </v-btn>
              </v-flex>
            </v-card-actions>
          </v-card>
        </v-col>
      </v-row>
    </v-container>


    <div v-else class="sin-datos">
      <p> No hay recorridos registrados todavia </p>
      <v-btn
          text
          color="primary">
        <v-icon left>
          mdi-plus
        </v-icon>
        <router-link to="/ingresar" class="btn">Ingresar</router-link>
      </v-btn>

    </div>







  </div>
</template>

<script>
export default {
  name: "Recorrido",

  data: function() {
    return {
      array: [],
      km: 0,


    }
  },
  mounted: function() {
    this.levantarArray();
    this.array = JSON.parse(localStorage.getItem("data")) || [];

  },

  methods: {

    formatDate: function(input) {
      let datePart = input.match(/\d+/g),
          year = datePart[0].substring(2),
          month = datePart[1],
          day = datePart[2];

      return day + '/' + month + '/' + year;
    },


    levantarArray: function() {
      if (localStorage.data) {
        this.array = JSON.parse(localStorage.getItem("data"));
      }

    },

    borrar: function(index) {
    let eliminar = confirm( `¿Deseas eliminar el recorrido ${this.array[index].nombre}?`);

      if (eliminar) {
        this.array.splice(index, 1);
        localStorage.setItem("data", JSON.stringify(this.array));
        return;
      }

    }

  },
}

</script>

<style>

.contenedor h1 {
  margin-bottom: 1em;
}

@media screen and (max-width: 576px) {
  div.col-md-6.col-lg-4.col-12 {
    padding-left: 0 !important;
  }
}

.ciudad {
  background: url('../../public/ciudad.jpg') no-repeat center center !important;
}

.montania {
  background: url('../../public/montania.jpg') no-repeat center center !important;}

.sin-datos {
  text-align: center;
  margin: 3em auto;
}

.sin-datos a:hover {
  color: black;
}

.editar {
  text-decoration: none;
}
a {
  text-decoration: none;

}

</style>