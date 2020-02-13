<template >
<v-app >
    <v-row >
      <v-col cols="6" xl="12" fluid fill-height style="background-color: #2195f2; font-family: 'Roboto Mono', monospace;" >
        <h1>Registrate para encontrar Hoteles muy cool</h1>
      </v-col>
      <v-col cols="6" xl="12" fluid fill-height align="center" style="font-family: 'Roboto Mono', monospace;">
          <v-text-field
            label="Solo"
            placeholder="Nombre"
            clearable
            solo
            rounded
            type="text"
            :rules="nameRules"
            v-model="name"
          ></v-text-field>
          <v-text-field
            label="Solo"
            placeholder="E-mail"
            solo
            clearable
            rounded
            :rules="emailRules"
            v-model="email"
          ></v-text-field>
          <v-text-field
            label="Solo"
            placeholder="Contraseña"
            solo
            clearable
            rounded
            :rules="passwordRules"
            type="password"
            v-model="password"
          ></v-text-field>
          <v-select
          :items="items"
          label="Selecciona el tipo de cuenta"
          dense
          solo
          rounded
          v-model="tipo"
        ></v-select>
        <v-btn rounded color="primary" dark v-on:click="usuarios">Crear Cuenta</v-btn>
        <v-col v-if="cuentaduplicada">
          <v-alert type="error">
            Ese correo ya esta registrado con otra cuenta
          </v-alert>
        </v-col>
      </v-col>
    </v-row>
</v-app>
</template>

<script>
import firebase from '../firebaseConfig'
export default {
  data () {
    return {
      cuentaduplicada: false,
      email: null,
      password: null,
      name: null,
      tipo: null,
      passwordRules: [
        v => !!v || 'Password is required'
      ],
      emailRules: [
        v => !!v || 'E-mail is required',
        v => /.+@.+/.test(v) || 'E-mail must be valid'
      ],
      nameRules: [
        v => !!v || 'Name is required'
      ],
      items: ['Hotelero', 'Viajero']
    }
  },
  methods: {
    agregarC () {
      var myname = this.name
      var tipo = this.tipo
      firebase.auth().createUserWithEmailAndPassword(this.email, this.password).then(() => {
        this.$router.push('/inicio')
      }).catch((error) => {
        // Handle Errors here.
        console.log(error)
        this.cuentaduplicada = true
      })
      firebase.auth().onAuthStateChanged(function (user) {
        if (user) {
          console.log('************************')
          console.log(myname)
          console.log('************************')
          console.log(user)
          // User is signed in.
          var displayName = user.displayName
          console.log(displayName)
          var email = user.email
          console.log(email)
          var emailVerified = user.emailVerified
          console.log(emailVerified)
          var photoURL = user.photoURL
          console.log(photoURL)
          var isAnonymous = user.isAnonymous
          console.log(isAnonymous)
          var uid = user.uid
          console.log(uid)
          var providerData = user.providerData
          console.log(providerData)
          var type = user.tipo
          console.log(type)
          // ...
          firebase.firestore().collection('usuarios').doc(uid).set({
            name: myname,
            email: email,
            tipo: tipo
          })
          // ...
          var user1 = firebase.auth().currentUser
          user1.sendEmailVerification().then(function () {
            // Email sent.
          })
        } else {
          // User is signed out.
          // ...
        }
      })
    },
    usuarios () {
      this.agregarC()
    }
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css?family=Roboto+Mono&display=swap');
</style>
