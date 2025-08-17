<script setup>
</script>

<template>
  <div class="barra">
    <div class="item">
        <button>
            <img src="@/assets/menu.png" width="30" height="30"/>
        </button>
    </div>
    <div class="item">
        <h3>My Weather App</h3>
    </div>
    <div class="item">
        <button>
            <img src="@/assets/plus.png" width="30" height="30"/>
        </button>
    </div>
  </div>
</template>

<style scoped>
  .barra{
    position: fixed;
    display: flex;
    top: 0;            /* Desde la parte superior */
    left: 0;           /* Desde la izquierda */
    width: 100%;       /* Ocupa todo el ancho */
    justify-content: space-between;
    align-items: center;
    background-color: rgb(78, 169, 226);
  }

  .item{
    margin-left: 10px;
    margin-right: 10px;
  }

  button{
    background-color: rgb(78, 169, 226);;
  }
</style>