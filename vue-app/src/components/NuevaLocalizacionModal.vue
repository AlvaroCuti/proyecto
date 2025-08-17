<script setup>
import { ref, onMounted, onBeforeUnmount } from 'vue'

const emit = defineEmits(['close', 'submit'])
const city = ref('')

function cerrar () {
  emit('close')
}

async function submit() {
  const nuevaCiudad = city.value.trim()
  if (!nuevaCiudad) return

  try {
    const apiUrl = import.meta.env.VITE_API_URL
    const apiKey = import.meta.env.VITE_API_KEY
    const response = await fetch(`${apiUrl}weather?q=${encodeURIComponent(nuevaCiudad)}&appid=${apiKey}`)
    
    if (!response.ok) {
      alert("Ciudad no válida")
      return
    }

    const ciudadesGuardadas = JSON.parse(localStorage.getItem('ciudades') || '[]')

    if (!ciudadesGuardadas.includes(nuevaCiudad)) {
      ciudadesGuardadas.push(nuevaCiudad)
    }

    localStorage.setItem('ciudades', JSON.stringify(ciudadesGuardadas))
    emit('submit', { city: nuevaCiudad })
    city.value = ''
  } catch (error) {
    console.error(error)
    alert("Error al verificar la ciudad")
  }
}

function onEsc (e) {
  if (e.key === 'Escape') cerrar()
}
onMounted(() => window.addEventListener('keydown', onEsc))
onBeforeUnmount(() => window.removeEventListener('keydown', onEsc))
</script>

<template>
  <div class="modal-backdrop" @click.self="cerrar">
    <div class="modal">
      <h2>Nueva localización</h2>
      <div class="ciudad">
        <input
        v-model="city"
        type="text"
        placeholder="Ej. Madrid"
        class="input"
      />
      </div>
      
      <div class="botones">
        <button
          type="button"
          class="btn primary"
          @click="submit"
          :disabled="!city.trim()"
        >
          Guardar
        </button>
        <button type="button" class="btn cancel" @click="cerrar">Cancelar</button>

      </div>
    </div>
  </div>
</template>

<style scoped>

.modal-backdrop{
  position: fixed;
  inset: 0;
  background: rgba(0,0,0,.5);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 1000;
  color: black;
}

.modal{
  background: #fff;
  border-radius: 10px;
  padding: 30px 20px; /* padding más pequeño para móvil */
  width: 90%;           /* ancho relativo */
  max-width: 400px;     /* ancho máximo */
  max-height: 90vh;     /* que no sobrepase altura de la pantalla */
  box-shadow: 0 10px 30px rgba(0,0,0,.2);
  display: flex;
  flex-direction: column;
  overflow-y: auto;     /* habilita scroll si el contenido es muy alto */
}

.modal > h2{
  font-family: 'Montserrat', sans-serif;
  font-size: 1.7rem;
}

.input{
  width: 100%;
  padding: 10px 12px;
  border: 1px solid #ddd;
  border-radius: 8px;
  outline: none;
  background-color: #fff;
  color: black;
  font-family: 'Montserrat', sans-serif;
  font-size: 1.2rem;
}

.botones{
  margin-top: 30px;
  display: flex;
  gap: 15px;
  justify-content: center;
}

.btn{
  padding: 8px 12px;
  border-radius: 8px;
  border: 1px solid #ccc;
  background: #f6f6f6;
  cursor: pointer;
}

.btn.primary{
  background: rgb(78, 169, 226);
  border-color: rgb(78, 169, 226);
  color: #fff;
}

.btn.cancel{
  background: #b62b2f;
  border-color: #b62b2f;
  color: #fff; 
}

.btn:disabled{
  opacity: .6;
  cursor: not-allowed;
}

.ciudad{
    margin-right: 20px;
}

@media (max-width: 480px){
  .modal {
    padding: 20px 15px;
    margin-left: 15px;
    margin-right: 15px;
  }
  .modal > h2 {
    font-size: 1.5rem;
  }
  .input {
    font-size: 1rem;
    padding: 8px 10px;
  }
  .botones {
    flex-direction: column;
    gap: 10px;
  }
  .btn {
    width: 100%;
    padding: 10px 0;
  }
}

</style>
