<template>
<v-app>
  <v-row>
    <v-col style="background-color:#2195f2" cols="6">
      <span><h1 align="center"> Hola de nuevo</h1></span>
    </v-col>
    <v-col align="center" cols="6">
      <v-text-field
            label="Solo"
            placeholder="E-mail"
            solo
            clearable="true"
            rounded
            :rules="emailRules"
            v-model="email"
      ></v-text-field>
      <v-text-field
            label="Solo"
            placeholder="Contraseña"
            solo
            clearable="true"
            rounded
            :rules="passwordRules"
            type="password"
            v-model="password"
      ></v-text-field>
      <v-btn rounded color="primary" dark v-on:click="agregarC">Iniciar Sesion</v-btn>
      <v-col v-if="errorAlIniciar">
          <v-alert type="error">
           El correo o la contraseña son incorrectos
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
      errorAlIniciar: false,
      email: null,
      password: null,
      passwordRules: [
        v => !!v || 'Password is required',
        v => v.length <= 6 || 'Password must be less than 6 characters'
      ],
      emailRules: [
        v => !!v || 'E-mail is required',
        v => /.+@.+/.test(v) || 'E-mail must be valid'
      ]
    }
  },
  methods: {
    agregarC () {
      firebase.auth().signInWithEmailAndPassword(this.email, this.password).then(() => {
        this.$router.push('/inicio')
      }).catch((error) => {
        console.log(error)
        this.errorAlIniciar = true
        // ...
      })
      console.log('el usuario: ' + this.email + 'ha iniciado sesion')
    }
  }
}
</script>

<style>
#txti{
  background-color: #c2c2c2;
}
#detras{
  height: 100%;
}
</style>
