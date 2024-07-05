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
  <h3>Ordenar por</h3>
  <div class="filtros">
    <button v-for="opcion in filtros" :key="opcion.id" >
      {{opcion}}
    </button>
  </div>


  <!--Esta sección es para las tarjetas del software de vigilancia-->
  <div v-for="(grupoAnio, anio) in tarjetasOrdenadas" :key="anio" class="year-group">  
      <h2>{{ anio }}</h2>
        <div class="contenedor-tarjetas" >
          <div class="tarjeta" v-for="software in grupoAnio" :key="software.id">>
            <div class="azul">
              <p>{{software['software']}}</p>
              <p>{{software['anio']}}</p>
              <p>{{software['empresa_creadora']}}</p>
              <p>{{software['pais_creador']}}</p>
              <p>{{software['empresa_vendedora']}}</p>
              <p>{{software['costo']}}</p>
            </div>
            <div class="blanco">
              <p>{{software['descripcion_corta']}}</p>
              <button v-if="software['mas_info']">Más info</button>
              <button>Consulta contrato</button>
            </div>
          </div>
        </div>
  </div>

</template>

<script setup>
import { ref, computed, onMounted } from "vue";

//data de los softwares, generado por script python
const tarjetas = ref([
  {
    software:"Geomatrix",
    tipo_tecnologia:"abc",
    dependencia:"sep",
    empresa_vendedora:"empresa vendedora", 
    empresa_creadora:"empresa creadora",
    pais_creador:"Pais",
    anio: 2014,
    costo: 1000000,
    descripcion_corta:"Descripcion cortita",
    mas_info: 1,
    descripcion_larga: "aquí va un link",
    link_contrato:"google.com"
  },
  {
    software:"Sofrwadlfkmsdfre2",
    tipo_tecnologia:"abc",
    dependencia:"sep",
    empresa_vendedora:"empresa vendedora", 
    empresa_creadora:"empresa creadora",
    pais_creador:"Pais",
    anio: 2014,
    costo: 1000000,
    descripcion_corta:"sldkjfsldf",
    mas_info: 0,
    descripcion_larga: "aquí va un link",
    link_contrato:"google.com"
  },
  {
    software:"Otro software",
    tipo_tecnologia:"abc",
    dependencia:"sep",
    empresa_vendedora:"empresa vendedora", 
    empresa_creadora:"empresa creadora",
    pais_creador:"Pais",
    anio: 2015,
    costo: 1000000,
    descripcion_corta:"sldkjfsldf",
    mas_info: 0,
    descripcion_larga: "aquí va un link",
    link_contrato:"google.com"
  }
]);

//este valor cambia según el botón que se elija en el menú de opciones
//const filtroSeleccionado = ref('ninguno'); //anio, dependencia, costo, tipo, intrusividad, pais

const filtros = ref([
    "Año de contratación",
    "Dependencia gubernamental",
    "Costo de compra",
    "Tipo de tecnología",
    "Nivel de intrusividad",
    "País de empresa creadora"
]);

//TODO: NEXT es como categoria del ejemplo 
/*const dependencia_gob = computed(() => {
  const todasDependencias = tarjetas.value.map(tarjeta => tarjeta.dependencia);
  return [...new Set(todasDependencias)];
});*/

/*const tarjetasAnio = computed(() =>{
  let tarjetasFiltradas = tarjetas.value;
  const agrupaAnios = tarjetasFiltradas.reduce((acc, tarjeta) => {
    (acc[tarjeta.anio] = acc[tarjeta.anio] || []).push(tarjeta);
    return acc;
  }, {});
  return agrupaAnios
});
*/
const tarjetasOrdenadas = computed(() => {
  let tarjetasFiltradas = tarjetas.value;
  //devuelve diccionario de anios 
  //const result = {};
  
  const agrupaAnios = tarjetasFiltradas.reduce((acc, tarjeta) => {
    (acc[tarjeta.anio] = acc[tarjeta.anio] || []).push(tarjeta);
    return acc;
  }, {});

  return agrupaAnios;
});

/*function groupByCategory(cards) {
  return cards.reduce((acc, card) => {
    (acc[card.category] = acc[card.category] || []).push(card);
    return acc;
  }, {});
}*/

onMounted(() => {
  console.log('filtros.value');
});
</script>
<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped></style>
