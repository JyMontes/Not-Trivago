<template>
  <div fluid class="fill-height" id="fondo">
    <!--Con sesion activa -->
    <v-app-bar
      color="#2195f2"
      dark
      v-if="iniciado == true"
    >

      <v-toolbar-title ><span id="t1"><b>Not</b></span><span id="t2"><b>Trivago</b></span></v-toolbar-title>

      <v-spacer></v-spacer>

      <span><p>{{email}}</p></span>

      <v-btn
        rounded
        v-on:click="cerrarSesion"
        style="font-family: 'Roboto Mono', monospace;"
      >
        Cerrar Sesion
      </v-btn>
    </v-app-bar>
    <!--Sin sesion activa -->
    <v-app-bar
      color="#2195f2"
      dark
      v-else
    >

      <v-toolbar-title ><span id="t1"><b>Not</b></span><span id="t2"><b>Trivago</b></span></v-toolbar-title>

      <v-spacer></v-spacer>

      <v-btn style="font-family: 'Roboto Mono', monospace;" rounded href="../#/iniciarSesion">
        Iniciar Sesion
      </v-btn>

      <v-btn
        rounded
        href="../#/registrarse"
        style="font-family: 'Roboto Mono', monospace;"
      >
        Registrarse
      </v-btn>
    </v-app-bar>
    <!--Listo para la siguiente aventura -->
    <v-container>
      <v-row align="center">
        <v-col cols="6" id="textp"  >
          <h1>Listo para la siguiente aventura?</h1>
          <h3>Vamo a darle <br> Lorem ipsum dolor sit amet consectetur, adipisicing elit. Dicta at iusto iure vero facere, sapiente enim esse odit ducimus quos culpa dolorem minima incidunt quaerat nisi laborum eaque deserunt impedit.</h3>
        </v-col>
        <v-col cols="6" id="imgp" >
          <img src="https://images.unsplash.com/photo-1525967304942-5e269b2dab58?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=755&q=80" max-height="100%" alt="">
        </v-col>
      </v-row>
    </v-container>
    <br><br><br><br><br><br><br><br><br>
    <!--Muestra los hoteles -->
    <v-container v-if="iniciado == true">
      <v-row align="center">
        <v-col xl="12" cols="4" v-for="pr in hoteles[0].length" v-bind:key="pr" >
          <v-hover  v-slot:default="{ hover }">
            <v-card
            class="mx-auto"
            max-width="400"
            :elevation="hover ? 24:2"
            color="#6ec5ff"
            >
            <v-img
              class="white--text align-end"
              height="200px"
              :src="hoteles[0][pr-1].img"
            >
            </v-img>
            <v-card-text class="text--primary">
              <v-card-title id="texto" style="background-color:#0068bf"><strong>{{hoteles[0][pr-1].name}}</strong> </v-card-title>
              <v-row>
                  <v-rating
                  v-model="rating"
                  color="amber"
                  half-increments
                  dense
                  hover="true"
                ></v-rating><br>
                <div class="grey--text ml-4">{{rating}}</div>
              </v-row>
              <div id="texto2"><strong>{{hoteles[0][pr-1].description}}</strong> </div>
              <div id="texto2"><strong>{{`$${hoteles[0][pr-1].price}`}}</strong> </div>
            </v-card-text>
             <v-card-actions>
              <v-btn
                text
                style="font-family: 'Roboto Mono', monospace; color:maroon"
              >
                Reservar
              </v-btn>
            </v-card-actions>
            </v-card>
          </v-hover>
        </v-col>
      </v-row>
    </v-container>
    <!--Si es hotelero -->
    <v-container v-if="hotelero == true">
      <v-row align="center">
         <v-btn
            text
            style="font-family: 'Roboto Mono', monospace;"
            href="../#/agregarHoteles"
          >
           Agrega tu hotel
         </v-btn>
      </v-row>
    </v-container>
  </div>
</template>

<script>
import firebase from '../firebaseConfig'
export default {
  data () {
    return {
      iniciado: false,
      hotelero: false,
      hoteles: [],
      email: null,
      ver: false,
      rating: 2
    }
  },
  methods: {
    obtenerUsuario () {
      var user = firebase.auth().currentUser
      if (user) {
        this.iniciado = true
        // User is signed in.
        this.email = user.email
        console.log(this.email)
        console.log(this.hotelero)
        // a continuacion vas a verificar si el usuario es hotelero
        console.log('*******************************')
        console.log(user)
        console.log('*******************************')
        if (user.tipo === 'Hotelero') {
          this.hotelero = true
          console.log('*******************************')
          console.log(this.hotelero)
          console.log('*******************************')
        }
        console.log(this.hotelero)
      } else {
        this.iniciado = false
        // No user is signed in.
      }
      var listaDeHoteles = []
      firebase.firestore().collection('hoteles').onSnapshot(function (querySnapshot) {
        querySnapshot.forEach(function (doc) {
          listaDeHoteles.push(doc.data())
          console.log('esta es lista de hoteles: ', listaDeHoteles)
        })
      })
      this.hoteles.push(listaDeHoteles)
      console.log('esta es hoteles: ', this.hoteles)
    },
    cerrarSesion () {
      firebase.auth().signOut().then(function () {
        // Sign-out successful.
        console.log('el usuario cerro su sesion')
      })
      console.log('el usuario cerro su sesion')
      this.iniciado = false
    }
  },
  created () {
    this.obtenerUsuario()
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css?family=Roboto+Mono&display=swap');
#texto{
  color: white;
  font-family: 'Roboto Mono', monospace;
}
#texto2{
  color:maroon;
  font-family: 'Roboto Mono', monospace;
}
#t1{
  color: #ff3d00;
  font-family: 'Roboto Mono', monospace;
}
#t2{
  color: white;
  font-family: 'Roboto Mono', monospace;
}
#textp{
  position: relative;
  z-index: 2;
  top: 100px;
  left: 100px;
  font-family: 'Roboto Mono', monospace;
}
#imgp{
  position: relative;
  z-index: 1;
  top: 100px;
  right: 200px;
}
</style>
