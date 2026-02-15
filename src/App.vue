<script setup>
import { ref } from 'vue'
import data from '../src/data/eventos.json'
import Tarjeta from './components/Tarjeta.vue';
import Nav from './components/Nav.vue';

const terminoBusqueda = ref('')
const mensajeBusqueda = ref('')
const eventLog = ref([])
const tipsMostrados = ref(false)
const modalVisible = ref(false)
/* const eventLog = ref([]) */
const MAX_LOG = 15

const eventos = ref(
  data.map(e => ({
    ...e,
    favorito: false
  }))
)

const resultados = ref([...eventos.value])

function buscar() {
  if (!terminoBusqueda.value.trim()) {
    mensajeBusqueda.value = 'Debe ingresar un término de búsqueda.'
    resultados.value = [...eventos.value]
    registrarEvento('buscar vacío')
    return
  }

  resultados.value = eventos.value.filter(e =>
    e.titulo.toLowerCase().includes(terminoBusqueda.value.toLowerCase())
  )

  mensajeBusqueda.value = resultados.value.length === 0
    ? 'No se encontraron resultados.'
    : ''

  registrarEvento(`buscar: ${terminoBusqueda.value}`)
}

function limpiar() {
  terminoBusqueda.value = ''
  resultados.value = [...eventos.value]
  mensajeBusqueda.value = ''
  registrarEvento('limpiar búsqueda')
}


function registrarEvento(texto) {
  const timestamp = new Date().toLocaleTimeString()

  eventLog.value.unshift(`[${timestamp}] ${texto}`)

  if (eventLog.value.length > MAX_LOG) {
    eventLog.value.pop()
  }
}

function limpiarLog() {
  eventLog.value = []
}

/* function registrarEvento(texto) {
  eventLog.value.unshift(texto)

  if (eventLog.value.length > 10) {
    eventLog.value.pop()
  }
} */

/* function seleccionarTarjeta(id) {
  registrarEvento(`click tarjeta ${id}`)
} */

function marcarFavorito(id) {
  const evento = eventos.value.find(e => e.id === id)
  if (evento) {
    evento.favorito = !evento.favorito
    registrarEvento(`favorito ${id}`)
  }
}

function logCaptura() {
  registrarEvento('captura section')
}

const tarjetaSeleccionada = ref(null)

function seleccionarTarjeta(id) {
  tarjetaSeleccionada.value = id
  registrarEvento(`click tarjeta ${id}`)
}

function mostrarTips() {
  tipsMostrados.value = true
  registrarEvento('mostrar tips')
}

const scrollPos = ref(0)

function onScroll(event) {
  scrollPos.value = event.target.scrollTop
  registrarEvento(`scroll: ${scrollPos.value}px`)
}

function abrirModalInfo() {
  modalVisible.value = true
  registrarEvento('abrir modal info')
}

</script>

<template>
  <Nav/>
  <h2>Cartelera de eventos</h2>
  <main>

    <section class="busqueda">
      <form @submit.prevent="buscar">

        <input v-model="terminoBusqueda" type="text" placeholder="Buscar evento..." @keyup.enter="buscar"
          @keydown.esc="limpiar" />

        <button type="submit" class="buscar-btn">
          Buscar
        </button>

        <!-- Ejemplo inline para mezclar estilos -->
        <button type="button" @click="terminoBusqueda = ''" class="limpiar-btn">
          Vaciar
        </button>

      </form>

      <p v-if="mensajeBusqueda">
        {{ mensajeBusqueda }}
      </p>
    </section>

    <section @click.capture="logCaptura" class="tarjetas">
      <Tarjeta v-for="evento in resultados" :key="evento.id" :evento="evento"
        :seleccionada="tarjetaSeleccionada === evento.id" @seleccionar="seleccionarTarjeta"
        @favorito="marcarFavorito" />

    </section>

    <section class="accion-unica">

  <button
    @click.once="mostrarTips"
    :disabled="tipsMostrados"
  >
    {{ tipsMostrados ? 'Tips ya mostrados' : 'Mostrar tips' }}
  </button>

  <div v-if="tipsMostrados" class="tips">
    <ul>
      <li>Llega con anticipación.</li>
      <li>Revisa la ubicación antes de salir.</li>
      <li>Lleva tu entrada digital lista.</li>
    </ul>
  </div>

</section>

<section class="info-link">

  <a
    href="https://example.com"
    @click.prevent="abrirModalInfo"
  >
    Más información del centro
  </a>

</section>

<div v-if="modalVisible" class="modal">
  <div class="modal-content">
    <p>Este centro organiza eventos culturales y tecnológicos durante todo el año.</p>

    <button @click="modalVisible = false">
      Cerrar
    </button>
  </div>
</div>

<section class="scroll-section">

  <p>Posición scroll: {{ scrollPos }} px</p>

  <div
    class="scroll-box"
    @scroll.passive="onScroll"
  >
    <div
      v-for="n in 40"
      :key="n"
      class="scroll-item"
    >
      Actualización {{ n }} del evento
    </div>
  </div>

</section>

<section class="audit-log">

  <div class="log-header">
    <h3>Registro de eventos</h3>
    <button @click="limpiarLog">
      Limpiar registro
    </button>
  </div>

  <ul v-if="eventLog.length">
    <li v-for="(evento, index) in eventLog"
        :key="index">
      {{ evento }}
    </li>
  </ul>

  <p v-else>
    Sin eventos registrados.
  </p>

</section>  

  </main>

</template>

<style>

h2{
  text-align: center;
  margin-block: 1em;
}

.busqueda form{
  margin: 1em auto;
  text-align: center;
}
.busqueda .buscar-btn, .limpiar-btn, input{
  padding: 0.5em 1em;
  border-radius: 5px;
  margin-inline-start: 0.5em;
}

.buscar-btn{
  background-color: rgb(198, 240, 125);
}

.limpiar-btn{
  background-color: rgb(236, 148, 148);
}

.tarjetas {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: 1em;
  padding: 2em;
}

.scroll-box {
  height: 200px;
  overflow: auto;
  border: 1px solid #ccc;
  padding: 10px;
}

.scroll-item {
  padding: 8px;
  border-bottom: 1px solid #eee;
}

.modal {
  position: fixed;
  inset: 0;
  background: rgba(0,0,0,0.5);
  display: flex;
  justify-content: center;
  align-items: center;
}

.modal-content {
  background: white;
  padding: 20px;
  border-radius: 8px;
}

.audit-log {
  margin-top: 20px;
  padding: 10px;
  border: 1px solid #ccc;
  max-height: 200px;
  overflow: auto;
  background: #fafafa;
}

.audit-log ul {
  list-style: none;
  padding: 0;
  margin: 0;
}

.audit-log li {
  font-size: 0.9rem;
  padding: 4px 0;
  border-bottom: 1px solid #eee;
}
</style>
