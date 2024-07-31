<template>
  <!--Esta sección es exclusivamente para la parte de la simbología-->
  <h3>Simbología</h3>
  <div class="simbologia">
    <div class="simbolo">
      <img src="../assets/logo.png" height="25" width="25" />
      <p>Dependencia por la que fue adquirido</p>
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
      <label :for="key" :class="{ 'selected-label': filtroSeleccionado === key }" class="radio-label">{{ value }}</label>
    </div>  
  </div>

  <p>{{hallazgos[filtroSeleccionado]}}</p>
  <!--Esta sección es para las tarjetas del software de vigilancia-->
  <div v-for="(tarjetasGrupales, grupo) in tarjetasFiltradas" :key="grupo" class="year-group">  
      <h2>{{ grupo }}</h2>
        <div class="cards" >
          <div class="card" v-for="software in tarjetasGrupales" :key="software.id">
            <div class="card-header">
              <h2 class="card-title">{{software['software']}}</h2>
              <div class="card-subtitle">
              <p class="card-subtitle">{{software['anio']}} | {{software['empresa_creadora']}} | {{software['pais_creador']}}</p> </div>
              <p>{{software['empresa_vendedora']}}</p>
              <p>{{software['costo']}}</p>
            </div>
            <div class="card-body">
              <p>{{software['descripcion_corta']}}</p>
            </div>
            <div class="card-buttons">
              <button v-if="software['mas_info']" @click="togglePopup(software)">Más info</button>
              <button @click="linkContrato(software['link_contrato'])">Consulta contrato</button>
            </div>
            <div class="popup-overlay" v-if="software['showPopup']">
              <div class="popup">
                <span class="close" @click="togglePopup(software)">&times;</span>
                <h2>{{ software['software'] }}</h2>
                <p>{{ software['info_extra']}}</p>
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
  
  //console.log(filtro, tarjetasFiltradas.value);
}

function linkContrato(link){
  const newWindow = window.open = (link, '_blank');
  if (!newWindow || newWindow.closed || typeof newWindow.closed === 'undefined') {
      window.location.href = link;
    }
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

p {
  margin:4px;
}

.cards {
  display: flex;
  flex-wrap: wrap;
  list-style: none;
  margin: 0;
  padding: 0;
}

.card {
  border: 1px solid #ccc;
  border-radius: 8px;
  overflow: hidden;
  width: 300px;
  margin: .5em;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}

.card-header {
  background-color: #CFD8F7; /* Light gray background */
  padding: 8px;
  height: 175px;
  font-family: Arial;
  font-weight: bold;
  font-size: 1em;
  
}

.card-body {
  background-color: #ffffff; /* White background */
  padding: 8px;
  height: 200px;
  overflow-y:auto;
}

.card-title {
  font-size: 1.3em;
}

.card-buttons {
  padding: 8px;
  background: #cfd8f7;
  display: flex;
  justify-content: center;
  gap: 00px;
}

button {
  font:inherit;
  padding: 10px 20px;
  margin: 4px;
  border-radius: 8px;
  justify-content: center;
	align-items: center;
	flex-shrink: 0;
	font-size: 1rem;
	transition: .25s ease;
	z-index: 1;
	cursor: pointer;
	color: #565656;
  border: 2px solid transparent;
  background-color: white;
  &:hover, &:focus {
		background-color: #abdbe3;
		color: #565656;
    outline: none;
	}

}

.card-subtitle {
  color: #6D6D6D;
}

.radio-menu {
  display: flex;
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
  background-color: #f0f0f0;
  transition: background-color 0.3s, border-color 0.3s;
}


.radio-label.selected-label {
  background-color: #007bff;
  color: white;
  border-color: #007bff;
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
  z-index:2;
}

.popup {
  background-color: white;
  padding: 20px;
  border-radius: 5px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.2);
  max-width: 80%;
  max-height: 80%;
  overflow: auto;
  position: relative;
  z-index:2;
}

.close {
  position: absolute;
  top: 10px;
  right: 15px;
  font-size: 24px;
  cursor: pointer;
  color: #aaa;
}

</style>
