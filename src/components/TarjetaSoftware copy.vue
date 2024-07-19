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

  <div class="radio-menu">
    <div
      v-for="(value, key, index) in filtros" :key="index"
      class="radio-button" >
      <input
        type="radio"
        :id="key"
        name= "filtros"
        @change = "filtrar"
        class="radio-input"
      />
      <label :for="key" class="radio-label">{{ value }}</label>
    </div>
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
const tarjetas = ref([{"software":"Geomatrix","id_licencia":1.0,"tipo_tecnologia":"Geolocalización y rastreo","descripcion_corta":"Localiza la ubicación de un teléfono celular en tiempo real, ofrece acceso a detalles como: número imsi, país de residencia, nombre del operador, estatus del teléfono (ausente/disponible/ocupado), vista satelital del mapa y vista a nivel de calle a través de google street view del objetivo. Cuenta con un robot que puede crear una secuencia de geolocalizaciones y presentar un recorrido cronológico del objetivo, además de alertar cuando éste sale de una zona determinada.","empresa_creadora":"Rayzone Group","empresa_vendedora":"Neolinx de México","pais_creador":"Israel","dependencia":"Fiscalía del Estado de Jalisco","costo":"$7,772,000.00","anio":2019.0,"fuentes":"https://rayzone.com/","mas_info":0.0,"info_extra":null,"n_intrusividad":"Alto","link_contrato":"https://drive.google.com/file/d/1mYRo0mKwglk67bt1GAd6G2nbDV0WErEj/view?usp=share_link","showPopup":0},{"software":"UFED: 4 PC, MOBiledit","id_licencia":2.0,"tipo_tecnologia":"Extracción y decodificación de datos","descripcion_corta":"Acceso a dispositivos y extracción de datos. Así como la decodificación, análisis y agrupación de datos  en una vista de cronograma de conexiones entre personas, lugares y eventos.","empresa_creadora":"CELLEBRITE","empresa_vendedora":"Nuga Sys S.A. de C.V.","pais_creador":"Israel","dependencia":"Instituto Jalisciense de Ciencias Forenses","costo":"$87,613.61","anio":2019.0,"fuentes":null,"mas_info":1.0,"info_extra":"Este contrato se rastreo a partir de una solicitud en la Plataforma Nacional de Transparencia (Folios), por lo tanto, los contratos se encuentran reservados.  El número de licencias compradas determina el alcance y potencialidad del producto adquirido. Sobre el funcionamiento y uso de las licencias adquiridas para cada Software: UFED ULTIMATE-ONE: se utiliza para la extracción de datos, decodificación y agrupación en una vista de cronograma unificada para determinar las conexiones entre personas, lugares y eventos; UFED 4 PC: permite el acceso a teléfonos básicos, dispositivos móviles inteligentes, drones, tarjetas SIM, tarjetas SD, dispositivos GPS y otros, para recolectar y extraer información; a través de la ruptura de códigos de acceso, barreras de cifrado, contenido eliminado o desconocido.","n_intrusividad":"Alto","link_contrato":"https://drive.google.com/file/d/1nRf3E_N1kUulXSs0xQJZbL_bzD8g5RSl/view?usp=drive_link","showPopup":0},{"software":"Reservado","id_licencia":null,"tipo_tecnologia":null,"descripcion_corta":null,"empresa_creadora":null,"empresa_vendedora":"Nuga Sys S.A. de C.V.","pais_creador":"Reservado","dependencia":"Instituto Jalisciense de Ciencias Forenses","costo":"$124,999.98","anio":2019.0,"fuentes":"Cellebrite Physical Analyzer - Cellebrite","mas_info":1.0,"info_extra":"Este contrato se rastreo a partir de una solicitud en la Plataforma Nacional de Transparencia (Folios) Los contratos se encuentran reservados.","n_intrusividad":"Alto","link_contrato":"Contratos .pngk","showPopup":0},{"software":"Reservado","id_licencia":null,"tipo_tecnologia":null,"descripcion_corta":null,"empresa_creadora":null,"empresa_vendedora":"Nuga Sys S.A. de C.V.","pais_creador":"Reservado","dependencia":"Instituto Jalisciense de Ciencias Forenses","costo":"$2,375,557.04","anio":2019.0,"fuentes":"Cellebrite Physical Analyzer - Cellebrite","mas_info":1.0,"info_extra":"Este contrato se rastreo a partir de una solicitud en la Plataforma Nacional de Transparencia (Folios) Los contratos se encuentran reservados.","n_intrusividad":"Alto","link_contrato":"Contratos .pngk","showPopup":0},{"software":"Geomatrix","id_licencia":1.0,"tipo_tecnologia":"Geolocalización y rastreo","descripcion_corta":"Localiza la ubicación de un teléfono celular en tiempo real, ofrece acceso a detalles como: número imsi, país de residencia, nombre del operador, estatus del teléfono (ausente/disponible/ocupado), vista satelital del mapa y vista a nivel de calle a través de google street view del objetivo. Cuenta con un robot que puede crear una secuencia de geolocalizaciones y presentar un recorrido cronológico del objetivo, además de alertar cuando éste sale de una zona determinada.","empresa_creadora":"Rayzone Group","empresa_vendedora":"Neolinx de México","pais_creador":"Israel","dependencia":"Fiscalía del Estado de Jalisco","costo":"$34,374,744.00","anio":2020.0,"fuentes":"https://rayzone.com/","mas_info":0.0,"info_extra":null,"n_intrusividad":"Alto","link_contrato":"https://drive.google.com/file/d/1dmPoTXWWnMvseK01HICpuBDSkxj6uhWk/view?usp=share_link","showPopup":0},{"software":"Geomatrix","id_licencia":1.0,"tipo_tecnologia":"Geolocalización y rastreo","descripcion_corta":"Localiza la ubicación de un teléfono celular en tiempo real, ofrece acceso a detalles como: número imsi, país de residencia, nombre del operador, estatus del teléfono (ausente/disponible/ocupado), vista satelital del mapa y vista a nivel de calle a través de google street view del objetivo. Cuenta con un robot que puede crear una secuencia de geolocalizaciones y presentar un recorrido cronológico del objetivo, además de alertar cuando éste sale de una zona determinada.","empresa_creadora":"Rayzone Group","empresa_vendedora":"Neolinx de México","pais_creador":"Israel","dependencia":"Fiscalía del Estado de Jalisco","costo":"$7,494,180.00","anio":2020.0,"fuentes":"https://rayzone.com/","mas_info":0.0,"info_extra":null,"n_intrusividad":"Alto","link_contrato":"https://drive.google.com/file/d/122Ldkqu4pJRC_wzn57ox8etsYZ4USuUY/view?usp=share_link","showPopup":0},{"software":"Geomatrix","id_licencia":1.0,"tipo_tecnologia":"Geolocalización y rastreo","descripcion_corta":"Localiza la ubicación de un teléfono celular en tiempo real, ofrece acceso a detalles como: número imsi, país de residencia, nombre del operador, estatus del teléfono (ausente/disponible/ocupado), vista satelital del mapa y vista a nivel de calle a través de google street view del objetivo. Cuenta con un robot que puede crear una secuencia de geolocalizaciones y presentar un recorrido cronológico del objetivo, además de alertar cuando éste sale de una zona determinada.","empresa_creadora":"Rayzone Group","empresa_vendedora":"Neolinx de México","pais_creador":"Israel","dependencia":"Fiscalía del Estado de Jalisco","costo":"$3,756,218.62","anio":2020.0,"fuentes":"https://rayzone.com/","mas_info":0.0,"info_extra":null,"n_intrusividad":"Alto","link_contrato":"https://drive.google.com/file/d/1ZoZVJywze0DhHPbpgdOwaw-HHbJ6ZCZm/view?usp=share_link","showPopup":0},{"software":"UFED: 4PC ULTIMATE RENEWAL, PHYSICAL ANALIZER","id_licencia":2.0,"tipo_tecnologia":"Extracción, decodificación y visualización","descripcion_corta":"Extracción y decodificación de datos. Traduce, preserva, analiza y revisa información de 50 fuentes de datos. Permite ver los datos de la aplicación en formato nativo.","empresa_creadora":"CELLEBRITE","empresa_vendedora":"XEROLOGIX, S.A. DE C.V.","pais_creador":"Israel","dependencia":"Secretaría de Administración del Poder Ejecutivo del Estado de Jalisco/ Fiscalía Estatal","costo":"$2,816,930.08","anio":2020.0,"fuentes":null,"mas_info":1.0,"info_extra":"En el contrato no se específica para qué institución será destinado. Sin embargo, menciona que la Fiscalía de Jalisco tiene competencia de reclamar el uso de la licencia al proovedor. El número de licencias compradas determina el alcance y potencialidad del producto adquirido.","n_intrusividad":"Alto","link_contrato":"https://drive.google.com/file/d/18K1PP1VQBvxP-WX2Hm-jYLNHoPuJiSJZ/view?usp=drive_link","showPopup":0},{"software":"Geomatrix","id_licencia":1.0,"tipo_tecnologia":"Geolocalización y rastreo","descripcion_corta":"Localiza la ubicación de un teléfono celular en tiempo real, ofrece acceso a detalles como: número imsi, país de residencia, nombre del operador, estatus del teléfono (ausente/disponible/ocupado), vista satelital del mapa y vista a nivel de calle a través de google street view del objetivo. Cuenta con un robot que puede crear una secuencia de geolocalizaciones y presentar un recorrido cronológico del objetivo, además de alertar cuando éste sale de una zona determinada.","empresa_creadora":"Rayzone Group","empresa_vendedora":"Neolinx de México","pais_creador":"Israel","dependencia":"Comisión de Búsqueda de Personas Desaparecidas del Estado de Jalisco","costo":"$3,756,218.62","anio":2020.0,"fuentes":"https://rayzone.com/","mas_info":1.0,"info_extra":"Esta información se adquirió a través de la solicitud de información UT/0AST-SGG/1175/2024 en la que se específica que sólo se adquirió una licencia para el año 2020.","n_intrusividad":"Alto","link_contrato":"https://drive.google.com/file/d/1cSdWww_e3rvkcORFODjksYqzvmC6Z1ss/view?usp=sharing","showPopup":0},{"software":"Geomatrix","id_licencia":3.0,"tipo_tecnologia":"Geolocalización y rastreo","descripcion_corta":"Localiza la ubicación de un teléfono celular en tiempo real, ofrece acceso a detalles como: número imsi, país de residencia, nombre del operador, estatus del teléfono (ausente/disponible/ocupado), vista satelital del mapa y vista a nivel de calle a través de google street view del objetivo. Cuenta con un robot que puede crear una secuencia de geolocalizaciones y presentar un recorrido cronológico del objetivo, además de alertar cuando éste sale de una zona determinada.","empresa_creadora":"Rayzone Group","empresa_vendedora":"Neolinx de México","pais_creador":"Israel","dependencia":"Fiscalía del Estado de Jalisco","costo":"$15,824,669.32","anio":2021.0,"fuentes":"https://rayzone.com/","mas_info":0.0,"info_extra":null,"n_intrusividad":"Alto","link_contrato":"https://drive.google.com/file/d/1ncanG5o8V1hSnx1Dp2z7W89erhzyBZaE/view?usp=share_link","showPopup":0},{"software":"Geomatrix","id_licencia":1.0,"tipo_tecnologia":"Geolocalización y rastreo","descripcion_corta":"Localiza la ubicación de un teléfono celular en tiempo real, ofrece acceso a detalles como: número imsi, país de residencia, nombre del operador, estatus del teléfono (ausente/disponible/ocupado), vista satelital del mapa y vista a nivel de calle a través de google street view del objetivo. Cuenta con un robot que puede crear una secuencia de geolocalizaciones y presentar un recorrido cronológico del objetivo, además de alertar cuando éste sale de una zona determinada.","empresa_creadora":"Rayzone Group","empresa_vendedora":"Neolinx de México","pais_creador":"Israel","dependencia":"Fiscalía del Estado de Jalisco","costo":"$8,159,000.00","anio":2022.0,"fuentes":"https://rayzone.com/","mas_info":0.0,"info_extra":null,"n_intrusividad":"Alto","link_contrato":"https://drive.google.com/file/d/1zeJSWiU7pvYxt0POpY-hzE3SGRthJia7/view?usp=share_link","showPopup":0},{"software":"UFED: 4PC, MOBILedit Forensic","id_licencia":2.0,"tipo_tecnologia":"Extracción, decodificación y visualización de datos","descripcion_corta":"Extracción de todos los datos de un teléfono, incluyendo datos eliminados, historial de llamadas, contactos, mensajes de texto, mensajes multimedia, fotos, videos, grabaciones, elementos del calendario, recordatorios, notas, archivos de datos, contraseñas y datos de aplicaciones como Skype, Dropbox, Evernote, Facebook, WhatsApp, Viber, Signal, WeChat y otras más. Combina todos los datos encontrados, elimina los duplicados y los presenta todos en un informe completo y de fácil lectura.","empresa_creadora":"Compelson lab y\n \nCELLEBRITE","empresa_vendedora":"NUGASYS S.A. DE C.V","pais_creador":"Israel y República Checa","dependencia":"Instituto Jalisciense de Ciencias Forenses","costo":"$273,785.71","anio":2022.0,"fuentes":"https://www.mobiledit.com/ https://snsfortech.com/MOBILedit","mas_info":1.0,"info_extra":"Este contrato se rastreo a partir de una solicitud en la Plataforma Nacional de Transparencia (Folios), por lo tanto, los contratos se encuentran reservados. El número de licencias compradas determina el alcance y potencialidad del producto adquirido.","n_intrusividad":"Alto","link_contrato":"https://drive.google.com/file/d/1V3r916Uw_7CPECqglg8IyEJJS0LLK-EH/view?usp=drive_link","showPopup":0},{"software":"UFED 4PC, MOBILedit Forensic","id_licencia":2.0,"tipo_tecnologia":"Extracción, decodificación y visualización de datos","descripcion_corta":"Extracción de todos los datos de un teléfono, incluyendo datos eliminados, historial de llamadas, contactos, mensajes de texto, mensajes multimedia, fotos, videos, grabaciones, elementos del calendario, recordatorios, notas, archivos de datos, contraseñas y datos de aplicaciones como Skype, Dropbox, Evernote, Facebook, WhatsApp, Viber, Signal, WeChat y otras más. Combina todos los datos encontrados, elimina los duplicados y los presenta todos en un informe completo y de fácil lectura.","empresa_creadora":"Compelson lab y\n\n\nCELLEBRITE","empresa_vendedora":"Gama Sistemas, S.A. de C.V.","pais_creador":"Israel y República Checa","dependencia":"Instituto Jalisciense de Ciencias Forenses","costo":"$114,474.60","anio":2022.0,"fuentes":"https://apps.apple.com/us/app/mobiledit/id541065297 \n\nhttps://www.mobiledit.com/ \n\nhttps://www.mobiledit.com/mobiledit-forensic","mas_info":1.0,"info_extra":"Este contrato se rastreo a partir de una solicitud en la Plataforma Nacional de Transparencia (Folios), por lo tanto, los contratos se encuentran reservados. El número de licencias compradas determina el alcance y potencialidad del producto adquirido.","n_intrusividad":"Alto","link_contrato":"https://drive.google.com/file/d/1j-EUUD6msvCWzxISAk929xSdgdKraj_d/view?usp=drive_link","showPopup":0},{"software":"SecurOs Premium","id_licencia":1.0,"tipo_tecnologia":"Análisis de video y reconocimiento facial","descripcion_corta":"Análisticas de video avanzadas de hasta 20 servidores, 640 cámaras y 20 canales de análisis de video. Los administradores pueden agregar analíticas, como: Reconocimiento Facial, Reconocimiento de Placas y un conjunto de detectores de analíticas situacional.","empresa_creadora":"Intelligent Security Systems ISS","empresa_vendedora":"MIRACLE BUSSINESS NETWORK, S.A. DE C.V","pais_creador":"México","dependencia":"Secretaría General de Gobierno","costo":"$616,876.40","anio":2023.0,"fuentes":"https://es.issivs.com/securos-premium-2/","mas_info":0.0,"info_extra":null,"n_intrusividad":"Medio","link_contrato":"https://drive.google.com/file/d/1BZRgXox3lizspWnf6FzZHlERiQAjbcm0/view","showPopup":0},{"software":"Se desconoce nombre. Software de Extracción de Información y Licencia de Sistema de Extracción Forense","id_licencia":2.0,"tipo_tecnologia":"Extracción de datos","descripcion_corta":"Reservado","empresa_creadora":"Reservado","empresa_vendedora":"RAMIR, S.A. DE C.V.","pais_creador":"Reservado","dependencia":"Secretaría de Seguridad Jalisco (Consejo Estatal de Seguridad Pública)","costo":"$652, 152.00","anio":2023.0,"fuentes":"(http://ramir.com.mx)","mas_info":1.0,"info_extra":"El nombre del software, el nombre de la empresa creadora y su uso aparecen testados en el contrato. El número de licencias compradas determina el alcance y potencialidad del producto adquirido.","n_intrusividad":"Alto","link_contrato":"https://drive.google.com/file/d/1Ns0GlmEztchBZhY1v0NCmcfWN0BzqmEL/view?usp=drive_link","showPopup":0},{"software":"Geomátrix 2.0","id_licencia":1.0,"tipo_tecnologia":"Geolocalización y rastreo","descripcion_corta":"Capacidad para ubicar, rastrear y manipular de modo encubierto suscriptores GSM, UMTS y LTE (2G, 3G y 4G), puede enviar consultas y presentar la información de la ubicación del objetivo en tiempo real, en una vista satelital del mapa o a nivel de calle, mostrando: latitud y longitud, número IMSI, país de residencia, operador, LAC y Cell ID, estatus del teléfono.","empresa_creadora":"Rayzone Group","empresa_vendedora":"RAMIR, S.A. DE C.V.","pais_creador":"Israel","dependencia":"Fiscalía del Estado de Jalisco","costo":"$7,355,560.00","anio":2023.0,"fuentes":"(http://ramir.com.mx ) Guadaljara","mas_info":0.0,"info_extra":null,"n_intrusividad":"Alto","link_contrato":"https://drive.google.com/file/d/116twSpMPo2ak8kiZDRHLSRlgcRZW9mxa/view?usp=drive_link","showPopup":0},{"software":"Software diversos: Software especializado en Análisis Forense Digital; Software especializado, licencias Axiom Magnet, Axiom Computer y Axiom Smartphone; Software especializado, licencias de uso para programa de recuperación y extracción de archivos de video dvr examiner y metadatos; Software Especializado Análisis Forense Digital (fuentes digitales); Licencias Encase Forense.","id_licencia":5.0,"tipo_tecnologia":"Análisis forense digital y de datos abiertos","descripcion_corta":"Examinar datos de código abierto, recolectar datos digitales y de fuentes móviles, recuperar datos borrados, explorar el historial completo de un archivo, realizar análisis y presentar los descubrimientos en un formato.","empresa_creadora":"Magnet Forensics y Opentext","empresa_vendedora":"E TRANSFORMATION S. DE R.L. DE C.V.","pais_creador":"Canadá","dependencia":"Fiscalía del Estado de Jalisco","costo":"$2,178,111.02","anio":2023.0,"fuentes":"https://www.magnetforensics.com/products/magnet-axiom/?gad_source=1&gclid=CjwKCAjw9IayBhBJEiwAVuc3fiflTdFSU29fXUmHmZKi3m03M60LeaywtjjbKxGPJ31Y1caWgXY85hoCpugQAvD_BwE","mas_info":1.0,"info_extra":"Este contrato incluye la compra de cinco softwares y licencias de uso. El número de licencias compradas determina el alcance y potencialidad del producto adquirido.","n_intrusividad":"Medio","link_contrato":"https://drive.google.com/file/d/1AuCsSN8BMzwHMGGT6-gyVG1p60Brvb5_/view","showPopup":0},{"software":"Software diversos: IBM i2 Enterprise Insight Analysis Recommendation Engine; Social Networks Analysis MKCVI; Rosoka Text Analytics (RTA).","id_licencia":3.0,"tipo_tecnologia":null,"descripcion_corta":"Rosoka: En más de 200 idiomas, Rosoka analiza textos no estructurados; Identificar y extraer las entidades y relaciones importantes junto con su información relevante. \nRosoka realiza análisis geoespaciales y proporciona a los usuarios las coordenadas para su uso con el software de mapeo de su elección. \nLos análisis de sentimientos de Rosoka te dicen si el lenguaje es persuasivo o controlador, positivo o negativo, feliz o triste, o incluso qué tan probable es que un lector reaccione. \nEstas capacidades estrechamente integradas brindan la información que su organización necesita para tomar decisiones informadas.\n\nhttps://docs.i2group.com/release-material/eia/2.5.0/i2-eia-2.5.0.html \n\nSocial Networks Analysis MKCVI: Visualizamos vínculos, correlacionamos datos, analizamos patrones y comportamientos.\n\nMostramos esta información por medio de gráficos y diagramas, utilizando entidades y enlaces para resaltar las relaciones.\n\nBuscamos a través de consultas visuales y definimos las dimensiones a analizar, construyendo cuadros de mando con KPIs o métricas adaptadas a la realidad de nuestros clientes. A continuación, dibujamos nuevas representaciones gráficas para abordar casos concretos, y monitorizamos esa información.\n\nEnterprise: Conversor de datos: i2 Enterprise Insight Analysis es una solución abierta, interoperable, ampliable y escalable que permite a las organizaciones convertir datos abrumadores y dispares en conocimientos e inteligencia procesables, casi en tiempo real.\n\ni2 Enterprise Insight Analysis ayuda a los analistas e investigadores a desarrollar una comprensión integral del panorama de amenazas para identificar vulnerabilidades e interrumpir las amenazas. La combinación de análisis dirigidos por máquinas y capacidades de análisis dirigidos por humanos ayuda a encontrar conexiones ocultas y patrones críticos que están enterrados en datos internos, externos y de código abierto.","empresa_creadora":"IBM i2","empresa_vendedora":"CUATRO NETWORKS, S. DE R.L. DE C.V.","pais_creador":"Reino Unido","dependencia":"Fiscalía del Estado de Jalisco","costo":"$1,934,629.65","anio":2023.0,"fuentes":"(https://4n.com.mx/)","mas_info":null,"info_extra":null,"n_intrusividad":null,"link_contrato":"https://drive.google.com/file/d/1vrT8LOwTkssQCwLy5CWeFhodS57wsbMj/view?usp=drive_link","showPopup":0},{"software":"Sofware de Geolocalización y Análisis Forense de imágenes","id_licencia":2.0,"tipo_tecnologia":"Geolocalización y Análisis Forense de imágenes","descripcion_corta":"Reservado","empresa_creadora":"Reservado","empresa_vendedora":"RAMIR, S.A. DE C.V.","pais_creador":"Reservado","dependencia":"Secretaría de Seguridad Pública (Consejo Estatal de Seguridad Pública)","costo":"$660,446.00","anio":2023.0,"fuentes":null,"mas_info":1.0,"info_extra":"El nombre del software, el nombre de la empresa creadora y su uso aparecen testados en el contrato. El número de licencias compradas determina el alcance y potencialidad del producto adquirido.","n_intrusividad":"Alto","link_contrato":"https://drive.google.com/file/d/1tm5upoeO2sIfkyNfYshHFFM-RWr63Qr9/view?usp=drive_link","showPopup":0},{"software":"Sofware Intellectus","id_licencia":1.0,"tipo_tecnologia":"Geolocalización y rastreo","descripcion_corta":"Permite ubicar y rastrear a suscriptores GSM, UMTS y LTE (2G, 3G y 4G) en la mayor parte del mundo que tenga cobertura con telefonía celular.","empresa_creadora":"Reservado","empresa_vendedora":"RAMIR, S.A. DE C.V. (http://ramir.com.mx)","pais_creador":"Reservado","dependencia":"Fiscalía del Estado de Jalisco","costo":"$28,337,408.00","anio":2023.0,"fuentes":"Nota: https://lajornadahidalgo.com/destinan-4-3-mdp-para-software-espia-de-pgjeh/ Contrato: https://transparencia.hidalgo.gob.mx/descargables/dependencias/OMayor/48/2019/DGCP/Tercer_trimestre/359-19.pdf","mas_info":0.0,"info_extra":null,"n_intrusividad":"Alto","link_contrato":"https://drive.google.com/file/d/1qfCIKfIL0C0zrfUqdK8Pla1oZncPLJ_A/view?usp=drive_link","showPopup":0}]);

//este valor cambia según el botón que se elija en el menú de opciones
const filtroSeleccionado = ref('ninguno'); //anio, dependencia, costo, tipo, intrusividad, pais

// Method to handle option selection
const seleccionarFiltro = (filtro) => {
  filtroSeleccionado.value = filtro;
  console.log(filtroSeleccionado.value);
};

const filtros = ref([
    "Año de contratación",
    "Dependencia gubernamental",
    "Costo de compra",
    "Tipo de tecnología",
    "Nivel de intrusividad",
    "País de empresa creadora"
]);

const filtros_dict = ref([
    "Año de contratación": "anio",
    "Dependencia gubernamental": "dependencia",
    "Costo de compra": "costo",
    "Tipo de tecnología": "tipo",
    "Nivel de intrusividad": "intrusividad",
    "País de empresa creadora": "pais"
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
  console.log(filtroSeleccionado);  
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
<style scoped>
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

.radio-button.selected .radio-label {
  background-color: #007bff;
  border-color: #007bff;
  color: white;
}

</style>


<!--  <h3>Ordenar por</h3>

  <div class="radio-menu">
    <div
      v-for="(value, key, index) in filtros" :key="index"
      class="radio-button" >
      <input
        type="radio"
        :id="key"
        name="filtros"
        @change="filtrarTarjetas(key)"

        class="radio-input"
      />
      <label :for="key" class="radio-label">{{ value }}</label>
    </div>
  </div>-->
  