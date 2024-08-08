<template>
  <!--Esta sección es exclusivamente para la parte de la simbología-->
  <link href="https://fonts.googleapis.com/css2?family=Open+Sans:wght@400;600;700&display=swap" rel="stylesheet">

    <h3 class="seccion">Simbología</h3>
  <div class="simbologia">
    <div class="simbolo">
      <img src="../assets/bank.png" height="25" width="25" />
      <p>Dependencia por la que fue adquirido</p>
    </div>
    <div class="simbolo">
      <img src="../assets/building.png" height="25" width="25" />
      <p>Empresa vendedora</p>
    </div>
    <div class="simbolo">
      <img src="../assets/paper.png" height="25" width="25" />
      <p>Costo de compra</p>
    </div>
  </div>

  <!--Esta sección es exclusivamente para los controles-->
  <!--Esta sección es exclusivamente para los controles-->
  <!--Creamos un botón por filtro que al ser presionado llame la función que actualiza la variable con la data filtrada-->
  <h3>Ordenar por</h3>

  <div class="radio-menu">
    <div class="radio-button" v-for="(value, key, index) in filtros" :key="index">
      <input 
          class="radio-input" 
          type="radio" 
          :id="key" 
          :value="key"
          :checked = "filtroSeleccionado === key"
          @click = "seleccionarFiltro(key)"
      />
      <label 
        :for="key" 
        :class="{ 'selected-label': filtroSeleccionado === key }" 
        class="radio-label">
        {{ value }}</label>
    </div>  
  </div>

  <!--Primero agregamos el hallazgo-->
  <p class="hallazgo" v-if="hallazgos[filtroSeleccionado] != null">
    {{ hallazgos[filtroSeleccionado] }}
  </p>
  <!--Esta sección es para las tarjetas del software de vigilancia-->
  <div v-for="grupo in listaFiltros" :key="grupo" class="grupo-tarjetas">
    <h2 class="categoria">{{ grupo }}</h2>
    <div class="cards">
      <div
        class="card"
        v-for="software in tarjetasFiltradas[grupo]"
        :key="software.id"
      >
        <div class="card-header">
          <!--Esta sección corresponde al header, solo al titulo y subtitulo-->
          <h2 class="card-title">{{ software["software"] }}</h2>
          <p class="card-subtitle">
            {{ software["anio"] }} | {{ software["empresa_creadora"] }} |
            {{ software["pais_creador"] }}
          </p>

          <!--Esta sección corresponde al header, al texto con los iconos-->
          <div class="info-header">
            <img src="../assets/bank.png" height="20" />
            <p>{{ software["dependencia"] }}</p>
          </div>
          <div class="info-header">
            <img src="../assets/building.png" height="20" />
            <p>{{ software["empresa_vendedora"] }}</p>
          </div>
          <div class="info-header">
            <img src="../assets/paper.png" height="20" />
            <p>{{ software["costo"] }}</p>
          </div>
          <button
              v-if="software['link_contrato']"
              @click="openLink(software['link_contrato'])"
              :href="software['link_contrato']"
              target="_blank"
              class="consulta-button-header"
              >Consulta contrato
            </button>
        </div>

        <!--Este es el recuadro blanco-->
        <div class="card-body">
          <p>{{ software["descripcion_corta"] }}</p>
          <div v-if="software['mas_info']" class="extra-info-buttons-container">
            <button
              v-if="software['mas_info']"
              @click="togglePopup(software)"
              class="extra-info-buttons"
            >
              Más info
            </button>
          </div>
        </div>
        <!--Esta es la sección de más info-->

        <div class="popup-overlay" v-if="software['showPopup']">
          <div class="popup">
            <div class="popup-header">
              <!--Esta sección corresponde al header, al texto con los iconos-->
              <h2 class="card-title">{{ software["software"] }}</h2>
              <p class="popup-subtitle">
                {{ software["anio"] }} | {{ software["empresa_creadora"] }} |
                {{ software["pais_creador"] }}
              </p>
              <div class="info-header">
                <img src="../assets/bank.png" height="20" />
                <p>{{ software["dependencia"] }}</p>
              </div>
              <div class="info-header">
                <img src="../assets/building.png" height="20" />
                <p>{{ software["empresa_vendedora"] }}</p>
              </div>
              <div class="info-header">
                <img src="../assets/paper.png" height="20" />
                <p>{{ software["costo"] }}</p>
              </div>
            </div>
            <span class="close" @click="togglePopup(software)">&times;</span>
            <div class="popup-body">
              <p>{{ software["info_extra"] }}</p>
              <div class="popup-button">
                <a
                  :href="software['link_contrato']"
                  target="_blank"
                  class="extra-info-buttons"
                  >Consulta contrato
                </a>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from "vue";
import tarjetasArchivo from '@/assets/tarjetas_archivo.json';
import hallazgosArchivo from '@/assets/hallazgos_archivo.json'
//data de los filtrados, generado por python
const tarjetas = ref(tarjetasArchivo);
const hallazgos = ref(hallazgosArchivo);

//data de los softwares, generado por script python
//const tarjetas = tarjetasArchivo;
const tarjetasFiltradas = ref();
var filtroSeleccionado = ref(null);
const listaFiltros = ref();

/*const ordenCorrecto = [
  "Más de 10 millones de pesos mexicanos", 
  "Entre de 5 y 10 millones de pesos mexicanos", 
  "Entre 1 y 5 millones de pesos mexicanos",
  "Entre medio y 1 millón de pesos mexicanos",
  "Menos de medio millón"
  ]*/

const filtros = ref({
  anio: "Año de contratación",
  dependencia: "Dependencia gubernamental",
  costos_cat: "Costo de compra",
  tipo_tecnologia: "Tipo de tecnología",
  pais_creador: "País de empresa creadora",
  n_intrusividad: "Nivel de intrusividad"
});

onMounted(() => {
  filtrarTarjetas(null);
});

function seleccionarFiltro(filtro){
  if (filtroSeleccionado.value === filtro) {
      filtroSeleccionado.value = null; // Deselect if the same radio button is clicked
  } else {
    filtroSeleccionado.value = filtro;
  }
  filtrarTarjetas(filtroSeleccionado);
}

function filtrarTarjetas(filtro) {
  //filtroSeleccionado = filtro;
  console.log("FILTRANDO TARJETAS");
  console.log(filtro)
  if(filtro == null || filtro.value == null){
    tarjetasFiltradas.value = {'Todos los softwares de vigilancia': tarjetas.value}
  }
  else {
  tarjetasFiltradas.value = tarjetas.value.reduce((acc, tarjeta) => {
      (acc[tarjeta[filtro.value]] = acc[tarjeta[filtro.value]] || []).push(tarjeta);
      return acc;
    }, {});
  }
  // Queremos dejar las categorías "Sin información", 'null' y 'Reservado' hasta el final
  // Entonces necesitamos una estructura ordenada
  // Vamos a obtener cada opción del filtro de manera programática,
  // pero para ordenar los costos vamos a usar card-code
  if (filtroSeleccionado.value == "costos_cat") {
    listaFiltros.value = [
      "Menos de medio millón",
      "Entre medio y 1 millón de pesos mexicanos",
      "Entre 1 y 5 millones de pesos mexicanos",
      "Entre de 5 y 10 millones de pesos mexicanos",
      "Más de 10 millones de pesos mexicanos",
    ];
  } else {
    listaFiltros.value = Object.keys(tarjetasFiltradas.value);
  }
  // Una vez creada la lista vamos a ordenarla
  // Ahora vamos a mandar al final de la lista la opción de Reservado
  if (listaFiltros.value.includes("Reservado")) {
    listaFiltros.value = listaFiltros.value.filter(
      (word) => word != "Reservado"
    );
    listaFiltros.value.push("Reservado");
  }
  // Después de Reservado iría la categoría "Sin información"
  if (listaFiltros.value.includes("Sin información")) {
    console.log("había sin información");
    listaFiltros.value = listaFiltros.value.filter(
      (word) => word != "Sin información"
    );
    listaFiltros.value.push("Sin información");
  }
  // Finalmente dejamos los que son null
  if (listaFiltros.value.includes("null")) {
    console.log("había null");
    listaFiltros.value = listaFiltros.value.filter((word) => word != "null");
    listaFiltros.value.push("null");
  }
  console.log(listaFiltros.value);

  //console.log(filtro, tarjetasFiltradas.value);
}

function openLink(link){
  //const newWindow = window.open = (link, '_blank');
  /*if (!newWindow || newWindow.closed || typeof newWindow.closed === 'undefined') {
      window.location.href = link;
    }*/
  window.open(link, '_blank');
}

function togglePopup(software){
  software.showPopup = !software.showPopup;
}

/*function adjustFontSizeHeader(){
  const header = cardHeader.value;
      const maxHeight = 150; // Fixed height of the header
      let fontSize = 1; // Starting font size, adjust as needed
      header.style.fontSize = fontSize + 'em';

      while (header.scrollHeight > maxHeight && fontSize > 10) { // Minimum font size threshold
        fontSize -= 1;
        header.style.fontSize = fontSize + 'px';
      }
}*/
</script>


<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
@import url('https://fonts.googleapis.com/css?family=Muli&display=swap');
.seccion {
  text-align: left;
}
.simbologia {
  display: flex;
  gap: 2%;
}
p {
  margin: 3px;
  text-align: left;
  font-size: 16px;
}

.grupo-tarjetas {
  padding: 10px;
}

.hallazgo {
  padding-left: 20px;
  padding-right: 20px;
  padding-top: 10px;
  margin-top: 20px;
  text-align: justify;
  font-size: 18px;
  font-weight: bold;
}

.categoria {
  text-align: left;
  margin-left: 15px;

  margin-bottom: 3px;
}
.cards {
  display: flex;
  flex-wrap: wrap;
  gap: 0.5%;
  justify-content: flex-start;
}

.card {
  border: 1px solid #ccc;
  border-radius: 8px;
  overflow: hidden;
  width: 320px;
  margin: 0.5em;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}

.card-header {
  display:flex;
  flex-direction: column;
  align-content: center;
  background-color: #FFF;
  padding: 8px;
  /*height: 280px;*/
  font-family: 'Muli', sans-serif;
  font-weight: bold;
  font-size: 1em;
}

.info-header {
  margin: 0px;
  padding: 0px;
  height: auto;
}
.card-body {
  background-color: #ffffff; /* White background */
  padding: 8px;
  height: 250px;
  overflow: scroll;
}

.card-title {
  font-size: 1.3em;
  margin-bottom: 10px;
  margin-top: 10px;
}

.card-subtitle {
  color: #195c4b;
  margin-bottom: 10px;
  text-align: center;
  margin-bottom: 15px;
}

.info-header {
  display: flex;
  align-items: center;
}

img {
  vertical-align: middle;
  margin: 3px;
}
.radio-menu {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 10px;
}

.radio-button {
  display: flex;
  align-items: center;
  cursor: pointer;
}

.radio-input {
  display: none;
}

.radio-label {
  padding: 10px 20px;
  border: 1px solid #ccc;
  border-radius: 5px;
  /* background-color: #f0f0f0;*/
  background-color: #d9d9d9;
  cursor: pointer;
  transition: background-color 0.3s, border-color 0.3s;
  font-size: 16px;
  font-weight: bold;
}

.radio-label.selected-label {
  background-color: #5294e0;
  border-color: #5294e0;
  /*     border-color: #007bff;
  background-color: #007bff; */
  color: white;
}
.popup-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
}

.popup {
  background-color: white;
  padding: 0px;
  border-radius: 5px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.2);
  max-width: 60%;
  max-height: 80%;
  overflow: auto;
  position: relative;
}

.popup-header {
  background-color: #cfd8f7;
  padding: 10px 40px;
}

.popup-body {
  padding: 20px 40px;
}

.popup-subtitle {
  color: #6d6d6d;
  font-weight: bold;
  text-align: center;
  margin-bottom: 15px;
}

.popup-button {
  display: flex;
  justify-content: flex-end;
  margin-top: 30px;
}

.close {
  position: absolute;
  top: 10px;
  right: 15px;
  font-size: 24px;
  cursor: pointer;
  color: #aaa;
}
.extra-info-buttons-container {
  display: flex;
  justify-content: space-evenly;
  margin: 15px;
}

.extra-info-buttons {
  border: 2px solid #ccc;
  border-radius: 5px;
  background-color: #d9d9d9;
  cursor: pointer;
  padding: 4px;
  font-weight: bold;
  font-size: 14px;
  color: black;
  text-decoration: none;
}
.consulta-button-header:hover,
.consulta-button-header:focus {
  background-color: #F1A805;
}

.consulta-button-header{
  
  border: 2px solid #ccc;
  border-radius: 5px;
  background-color: #F2D6A1;
  cursor: pointer;
  padding: 4px 16px;
  font-weight: bold;
  font-size: 14px;
  color: black;
  text-decoration: none;
  margin: 7px;
  align-self: left;
  margin-top: 15px;
  border: 1px solid #ccc;
  transition: background-color 0.3s, border-color 0.3s;
}

a:active,
a:active {
  color: #19578a;
}
@media (max-width: 769px) {
  .grupo-tarjetas {
    padding: 0px;
  }
  .card {
    width: 90%;
    margin: 15px;
  }
  .card-header {
    height: 200px;
  }

  .popup {
    max-width: 85%;
  }
}
</style>
