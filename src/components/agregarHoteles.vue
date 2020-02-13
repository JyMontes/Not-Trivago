<template>
  <v-container fluid fill-height id="fondo">
    <v-row style="text-align: center">
      <v-col><h1>Agrega Tu Hotel</h1></v-col>
    </v-row>
    <v-row >
        <v-col cols="12" xl="12">
          <v-text-field
            label="Solo"
            placeholder="Nombre del hotel"
            solo
            clearable="true"
            rounded
            :rules="nameRules"
            v-model="name"
          ></v-text-field>
          <v-text-field
            label="Solo"
            placeholder="Precio por habitacion"
            solo
            clearable="true"
            rounded
            :rules="priceRules"
            type="number"
            v-model="price"
          ></v-text-field>
        <v-textarea
          solo
          name="input-7-4"
          label="Descripcion del hotel"
          rounded
          clearable="true"
          counter="300"
          :rules="desRules"
          v-model="description"
        ></v-textarea>
        <v-text-field
            label="Solo"
            placeholder="URL de imagen del producto"
            solo
            clearable="true"
            rounded
            :rules="imgRules"
            v-model="img"
          ></v-text-field>
          <v-col align="center">
            <img :src="img" alt="" width="500px">
            <div class="text-center">
              <v-rating v-model="rating" half-increments hover color="#FFC107"></v-rating>
            </div>
          </v-col>
      </v-col>
      <v-col>
        <v-hover  v-slot:default="{ hover }">
          <v-col><v-btn rounded color="#EF5350"  :elevation="hover ? 12:2" v-on:click="crearHotel">Agregar Hotel</v-btn></v-col>
        </v-hover>
      </v-col>
      <v-col>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import shortId from 'shortid'
import firebase from '../firebaseConfig'
export default {
  data () {
    return {
      name: null,
      description: null,
      price: null,
      img: null,
      rating: [],
      nameRules: [
        v => !!v || 'Name is required'
      ],
      priceRules: [
        v => !!v || 'Price is required'
      ],
      imgRules: [
        v => !!v || 'Image is required'
      ],
      desRules: [
        v => !!v || 'Description is required'
      ]
    }
  },
  methods: {
    crearHotel () {
      var id = shortId.generate()
      firebase.firestore().collection('hoteles').doc(id).set({
        id: id,
        name: this.name,
        description: this.description,
        price: this.price,
        img: this.img,
        rating: this.rating
      })
    }
  }
}
</script>

<style>

</style>
