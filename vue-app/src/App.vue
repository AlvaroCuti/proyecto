<script setup>
import NavBar from './components/NavBar.vue'
import WeatherPlace from './components/WeatherPlace.vue'

const cityName = "Madrid" 
</script>

<template>
  <NavBar />
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
  top: 150px;            /* Desde la parte superior */
  left: 0;           /* Desde la izquierda */
  width: 100%;       /* Ocupa todo el ancho */
  height: 100%;
}

</style>
