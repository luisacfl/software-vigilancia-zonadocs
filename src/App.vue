<template>
  <!--Esta sección es la introducción-->
  <div class="header">
    <h1>Software de vigilancia en jalisco</h1>
    <p>
      Texto introductorio sobre los datos. <br />
      Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod
      tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim
      veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea
      commodo consequat. Duis aute irure dolor in reprehenderit in voluptate
      velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat
      cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id
      est laborum
    </p>
  </div>
  <h3>Investigación completa</h3>
  <button>Ilustración de la historia general</button>

  <!--Esta sección es para la viz-->
  <TarjetaSoftware />

  <!--Esta sección es para lo que sobra después de la viz-->
  <h3>Investigación completa</h3>
  <h1>Botones de historias con las ilustraciones</h1>
  <p>
    Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod
    tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam,
    quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo
    consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse
    cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat
    non proident, sunt in culpa qui officia deserunt mollit anim id est laborum
  </p>
</template>

<script setup>
import TarjetaSoftware from "./components/TarjetaSoftware.vue";
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: black;
  margin-top: 60px;
}

.simbolo {
  display: flex;
  align-items: center;
  /*justify-content: center;*/
}

.contenedor-tarjetas {
  margin: 20px;
  display: flex;
  flex-wrap: wrap;
  gap: 20px;
  background-color: #b8b8b8;
}

.tarjeta {
  width: 20%;
  padding: 10px;
  margin: 10px;
  background-color: white;
}
.azul {
  width: 100%;
  background-color: #7ba4b7;
}

.blanco {
  width: 100%;
}
</style>
