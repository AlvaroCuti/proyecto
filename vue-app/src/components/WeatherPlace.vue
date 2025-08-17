<script setup>
import { onMounted, ref } from 'vue'

const props = defineProps({
  city: {
    type: String,
    required: true
  }
})

const weatherData = ref(null);

const crear = async () => {
  try {
    const response = await fetch(`https://api.openweathermap.org/data/2.5/weather?q=Molina de Segura&units=metric&appid=287546363e58c3559cd8f54f329a2063`);

    if (response.ok) {
      const data = await response.json();
      weatherData.value = data;           
      console.log("Datos del clima:", data);
    } else {
      const errorData = await response.json();
      console.error("Error:", errorData);
    }
  } catch (error) {
    console.error("Error en la solicitud:", error);
  }
};

onMounted(() => {
  crear();
});
</script>

<template>
  <div class="completo">
    <div class="ciudad" v-if="weatherData">
      <h3>Weather of:</h3>
      <div class="nombre">
        <h1>{{ weatherData.name }}</h1>
        <h2>{{ weatherData.sys.country }}</h2>
      </div>
      <div class="datos">
        <div class="temp">
          <div class="info">
            <div class="imagen">
              <img 
                :src="`https://openweathermap.org/img/wn/${weatherData.weather[0].icon}@2x.png`" 
                :alt="weatherData.weather[0].description"
              />
              <span>{{ weatherData.weather[0].description }}</span>
            </div>
            
            <div class="datos_temp">
              <h1>{{ weatherData.main.temp }} ºC</h1>
              <h3>               
                <img src="@/assets/up-arrow.png" width="13" height="13"/>
                  {{ weatherData.main.temp_max }} ºC /               
                <img src="@/assets/down-arrow.png" width="13" height="13" tranform="rotate(180deg)"/>
                 {{ weatherData.main.temp_min }} ºC
              </h3>
            </div>
          </div>
        </div>
        <div class="dato_row">
          <span class="label">Pressure:</span>
          <span class="value">{{ weatherData.main.pressure }} hPa</span>
        </div>
        <div class="dato_row">
          <span class="label">Humidity:</span>
          <span class="value">{{ weatherData.main.humidity }} %</span>
        </div>
        <div class="dato_row">
          <span class="label">Visibility:</span>
          <span class="value">{{ weatherData.visibility }} m</span>
        </div>
        <div class="dato_row">
          <span class="label">Wind:</span>
          <span class="value">{{ weatherData.wind.speed }} m/s - {{ weatherData.wind.deg }}º</span>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.completo {
  display: flex;
  justify-content: flex-start;
  width: 100%;
  color: black;        
}

.ciudad{
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
  align-items: flex-start;
  padding-left: 50px;
}

.nombre{
  display: flex;
  flex-direction: row;
  justify-content: flex-start;
  column-gap: 10px;
}

.datos{
  display: flex;
  flex-direction: column;
  gap: 20px;
}

.temp{
  display: flex;
  flex-direction: column;
  align-items: flex-start;
}

.info{
  display: flex;
  flex-direction: row;
  align-items: center;
  gap: 42px;
}

.info > img {
  width: 120px;
  height: 120px;
  object-fit: contain;
  margin-bottom: 35px;
  margin-left: 10px;
}

.completo, .ciudad, .datos, .temp, .info {
  width: 100%;
}

.nombre {
  display: flex;
  flex-direction: row;
  align-items: center;
  column-gap: 18px;
}

.nombre > h1 {
  font-family: 'Montserrat', sans-serif;
  font-weight: 300;
  font-size: 3.5rem;
  margin: 0;
}

.nombre > h2 {
  font-family: 'Montserrat', sans-serif;
  font-weight: 400;
  font-size: 2.5rem;
  padding-top: 10px;
  color: rgb(156, 156, 156); 
}

.datos_temp > h3{
  color:rgb(156, 156, 156);
  font-weight: 500;
}

.imagen{
  display: flex;
  flex-direction: column;
}

.imagen > span{
  color:rgb(156, 156, 156);
  font-weight: 500;
  font-size: 1.25rem;
}

.dato_row {
  display: flex;
  justify-content: space-between;
  align-items: center;
  width: 94%;
  padding: 8px 0;
  border-bottom: 1px solid #ddd;
  font-family: 'Montserrat', sans-serif;
  font-size: 1.5rem;
  padding-bottom: 15px;
}

.dato_row:last-child {
  border-bottom: none;
}

.dato_row .label {
  text-align: left;
  font-weight: 700;
}

.dato_row .value {
  text-align: center;
  flex-grow: 1;
  font-weight: 400;
}

</style>
