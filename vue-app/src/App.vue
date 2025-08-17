<script setup>
import NavBar from './components/NavBar.vue'
import WeatherPlace from './components/WeatherPlace.vue'
import { ref } from 'vue'

const cityName = ref("Madrid")

function actualizarCiudad(nuevaCiudad) {
  cityName.value = nuevaCiudad
}
</script>

<template>
  <NavBar @city-selected="actualizarCiudad" />
  <div class="content">
    <WeatherPlace :city="cityName"/>
  </div>
</template>

<style>
body {
  margin: 0;
  padding: 0;
  height: 100%;
  background-color: white;
}

.content{
  position: fixed;
  display: flex;
  top: 150px;           
  left: 0;          
  width: 100%;      
  height: 100%;
}

</style>
