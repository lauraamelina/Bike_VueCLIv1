<template>
  <div>
    <v-carousel>
      <v-carousel-item
          v-for="(item,index) in items"
          :key="index"
          :src="item.src"
          reverse-transition="fade-transition"
          transition="fade-transition"
          height="23em"
      ></v-carousel-item>
    </v-carousel>
    <section class="text-center">
      <h1 class="text-center mt-6">Bienvenido a <span>VELOBIKE</span> </h1>
      <p class="pl-9 pr-9"> Ingresá todos tus recorridos en bicicleta y llevá un registro de cada uno. En Velobike <strong> queremos ayudarte a llevar una vida saludable. </strong> Ingresando todos los detalles del recorrido podés llevar un registro ordenado y dividido entre montaña y ciudad.</p>

      <v-btn
          text
          color="primary">
        <v-icon left>
          mdi-plus
        </v-icon>
        <router-link to="/ingresar" class="btn">Empezar con los recorridos</router-link>
      </v-btn>
    </section>

    <section class="beneficios text-center">
      <h2>Beneficios de andar en bicicleta</h2>
      <v-row class="mt-5">
        <v-col cols="12" md="4" >
            <v-card
                class="mx-auto"
                width="410"
            >
              <v-img
                  src="espalda.jpg"
                  height="200px"
              ></v-img>

              <v-card-title>
                Posición de la espalda
              </v-card-title>

              <v-card-subtitle>
                La inclinación de nuestro cuerpo hacia el manubrio favorece que los músculos del área lumbar se tensen y se refuercen, de esta forma, ganamos resistencia en esta parte de la espalda, ideal para estabilizar el tronco.
              </v-card-subtitle>
            </v-card>
        </v-col>
        <v-col cols="12" md="4" >
          <v-card
              class="mx-auto"
              width="410"
          >
            <v-img
                src="musculos.jpg"
                height="200px"
            ></v-img>

            <v-card-title>
              Fortalece músculos
            </v-card-title>

            <v-card-subtitle>
             Fortalece las rodillas, músculos y protege articulaciones. Además, al recaer la mayor parte del peso del cuerpo en el asiento, las rodillas no sufren, evitando que los cartílagos se desgasten de forma brusca.
            </v-card-subtitle>
          </v-card>
        </v-col>

        <v-col cols="12" md="4" >
          <v-card
              class="mx-auto"
              width="410"
          >
            <v-img
                src="estres.jpg"
                height="200px"
            ></v-img>

            <v-card-title>
              Reduce el estrés
            </v-card-title>

            <v-card-subtitle>
              El ejercicio físico permite oxigenar el cerebro y crear determinadas hormonas que ayudan a combatir estos estados negativos. Montar en bicicleta hace más felices y elimina muchos problemas psicológicos.
            </v-card-subtitle>
          </v-card>
        </v-col>

      </v-row>

    </section>
    <section class="mt-16">
      <h2>Algunos de los recorridos más conocidos</h2>
      <v-container>
        <v-row>
          <v-col cols="12" md="6" lg="4" v-for="(x,index) in recorridos" :key="index"
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

            </v-card>
          </v-col>
        </v-row>
      </v-container>
    </section>
  </div>

</template>
<script>


  export default {
    name: 'Home',

    data () {
    return {
      items: [
          {
          src: 'bicicleta-ruta.jpg',
          },
          {
          src: 'bici-urbana.jpg',
          },
      ],
      recorridos:[],
    }
  },
  mounted:function(){
    this.cargar();
    console.log(this.recorridos);
  },
  methods:{
    formatDate: function(input) {
      let datePart = input.match(/\d+/g),
          year = datePart[0].substring(2),
          month = datePart[1],
          day = datePart[2];

      return day + '/' + month + '/' + year;
    },
    cargar:function(){
        this.recorridos=JSON.parse(localStorage.getItem("recorridos"))
    },
  }

}
</script>
<style>
.beneficios {
  margin: 3em auto;
}

a {
  text-decoration: none;
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
  background: url('../../public/montania.jpg') no-repeat center center !important;
}



</style>