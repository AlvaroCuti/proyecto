<script setup>
import { ref, onMounted } from 'vue'
import NuevaLocalizacionModal from './NuevaLocalizacionModal.vue'

const modalVisible = ref(false)
const menuVisible = ref(false)
const ciudades = ref([])

const emit = defineEmits(['city-selected'])

function abrirModal () {
  modalVisible.value = true
}

function cerrarModal () {
  modalVisible.value = false
}

function guardarLocalizacion ({ city }) {
  const guardadas = JSON.parse(localStorage.getItem('ciudades') || '[]')
  if (!guardadas.includes(city)) {
    guardadas.push(city)
    localStorage.setItem('ciudades', JSON.stringify(guardadas))
    ciudades.value = guardadas
  }
  cerrarModal()
}

function onEsc (e) {
  if (e.key === 'Escape') cerrarModal()
}

onMounted(() => {
  ciudades.value = JSON.parse(localStorage.getItem('ciudades') || '[]')
})

function toggleMenu () {
  menuVisible.value = !menuVisible.value
}

function seleccionarCiudad(c) {
  emit('city-selected', c)  
  menuVisible.value = false
}

</script>

<template>
  <div class="barra">
    <div class="item">
      <button @click="toggleMenu">
        <img src="@/assets/menu.png" width="30" height="30"/>
      </button>
      <div v-if="menuVisible" class="menu-lista">
        <ul>
          <li v-for="c in ciudades" :key="c" @click="seleccionarCiudad(c)">
            {{ c }}
          </li>        
        </ul>
      </div>
    </div>

    <div class="item">
      <h3>My Weather App</h3>
    </div>

    <div class="item">
      <button @click="abrirModal">
        <img src="@/assets/plus.png" width="30" height="30"/>
      </button>
    </div>
  </div>

  <teleport to="body">
    <NuevaLocalizacionModal
      v-if="modalVisible"
      @close="cerrarModal"
      @submit="guardarLocalizacion"
    />
  </teleport>
</template>

<style scoped>
.barra{
  position: fixed;
  display: flex;
  top: 0;
  left: 0;
  width: 100%;
  justify-content: space-between;
  align-items: center;
  background-color: rgb(78, 169, 226);
  padding: 6px 10px;
  z-index: 10;
}

.item{
  margin-left: 10px;
  margin-right: 10px;
  position: relative;
}

button{
  background-color: rgb(78, 169, 226);
  border: none;
  cursor: pointer;
}

.menu-lista{
  position: absolute;
  top: 40px;
  left: 0;
  background: #fff;
  border: 1px solid #ddd;
  border-radius: 6px;
  box-shadow: 0 2px 6px rgba(0,0,0,.2);
  padding: 15px;
  z-index: 20;
  width: 200px;
  color: black;
}

.menu-lista ul{
  list-style: none;
  margin: 0;
  padding: 0;
  font-family: 'Montserrat', sans-serif;
  font-weight: 200;
  font-size: 1.2rem;
}
.menu-lista li{
  padding: 6px 10px;
  cursor: pointer;
}
.menu-lista li:hover{
  background: #f0f0f0;
}
</style>
