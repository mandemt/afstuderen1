<script setup>
import fetchdata from '../components/FetchData.vue'
import axios from 'axios'
import FormData from 'form-data'
import image from "../img/duizendknoop.png" // File System | Node.js
</script>

<template>
  <h1>Plantnet API. plantensoorten en identificeren</h1>
  <main>
    <h2>de data in kaart</h2>
    <button @click="fetchData">Alle plantnamen</button>
    <input type="file" @change="identifyPlant">identificeer plant</input>
    <h3 v-if="plantNaam">{{ plantNaam[0] }}</h3>
    <img src="../img/duizendknoop.png">

    <table v-if="kippie">
      <tr>
        <td>titel</td>
      </tr>
      <tr v-for="lo in kippie">
        <td>{{ lo.commonNames }}</td>

      </tr>
    </table>
  </main>
  <hr>
  <fetchdata />
</template>

<script>
let API_KEY = import.meta.env.VITE_API_KEY

export default {

  data() {
    return {
      titles: [],
      lol: [],
      kippie: [],
      plantNaam: null
    }
  },
  methods: {
    removeDuplicates(data) {
      const unique = [...new Map(data.map(item =>
        [item['id'], item])).values()];
      this.kippie = unique
    },

    async fetchData() {
      let response = await fetch("https://my-api.plantnet.org/v2/species?api-key=" + API_KEY + "&lang=nl")
      let rawData = await response.json()
      rawData.forEach(plant => {
        if (plant.commonNames.length !== 0) {
          this.lol.push(plant)
        }
      })
      this.removeDuplicates(this.lol)
    },

    async identifyPlant(e) {
      console.log('id')
      console.log(e.target.files)
      console.log(URL.createObjectURL(e.target.files[0]))
      let plaatje = URL.createObjectURL(e.target.files[0])
      const body = new FormData
      body.append("images", e.target.files[0])
      body.append("organs", "leaf")
      let response = await fetch("https://my-api.plantnet.org/v2/identify/all?include-related-images=false&no-reject=false&lang=nl&type=legacy&api-key=2b10Qo4Dc15tRmkloqUvnqe", {
        body,
        headers: {
          Accept: "image/*",
        },
        method: "POST"
      })
      let result = await response.json()
      this.plantNaam = result.results[0].species.commonNames
    },

  }




}


</script>
<style>
img {
  width: 100px;
  height: 100px;
}
</style>