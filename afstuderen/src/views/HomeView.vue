<script setup>
import TheWelcome from '../components/FetchData.vue'


// console.log(process.env.VUE_APP_PLANTNET_API_KEY)

let API_KEY = import.meta.env.VITE_API_KEY
console.log(API_KEY)
function fetchData() {
 const response = fetch("https://my-api.plantnet.org/v2/species?api-key=" + API_KEY + "&lang=nl")
const data = response.json
console.log(data)
}
</script>

<template>
  <main>
<button @click="fetchData">klik</button>
    <TheWelcome />
  </main>
</template>


