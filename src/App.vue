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
const MAX_LOG = 15
const tarjetaSeleccionada = ref(null)
const scrollPos = ref(0)
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

function seleccionarTarjeta(id) {
  tarjetaSeleccionada.value = id
  registrarEvento(`click tarjeta ${id}`)
}

function mostrarTips() {
  tipsMostrados.value = true
  registrarEvento('mostrar tips')
}

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
  <Nav />
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
      <button @click.once="mostrarTips" :disabled="tipsMostrados">
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
      <a href="https://example.com" @click.prevent="abrirModalInfo">
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
      <div class="scroll-box" @scroll.passive="onScroll">
        <div v-for="n in 40" :key="n" class="scroll-item">
          Actualización {{ n }} del evento
        </div>
      </div>
    </section>

    <section class="audit-log">
      <div class="log-header">
        <h3>Registro de eventos</h3>
        <button @click="limpiarLog" class="limpiar-btn">
          Limpiar registro
        </button>
      </div>
      <ul v-if="eventLog.length">
        <li v-for="(evento, index) in eventLog" :key="index">
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

h2 {
  text-align: center;
  margin-block: 1em;
}

.busqueda form {
  margin: 1em auto;
  text-align: center;
}

.busqueda .buscar-btn,
.limpiar-btn,
input {
  margin-inline-start: 0.5em;
}

.buscar-btn {
  background-color: rgb(198, 240, 125);
}

.limpiar-btn {
  background-color: rgb(236, 148, 148);
}

.tarjetas {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: 1em;
  padding: 2em;
}

.accion-unica,
.info-link,
.scroll-section,
.audit-log {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 1em;
}

.accion-unica {
  background-color: rgb(193, 211, 211);
  padding-block: 2em;
}

.accion-unica button {
  background-color: rgb(198, 240, 125);
}

.info-link {
  background-color: rgb(223, 208, 210);
  padding-block: 2em;
}

.scroll-section {
  background-color: rgb(221, 221, 208);
  padding-block: 2em;
}

.scroll-box {
  height: 200px;
  overflow: auto;
  border: 1px solid #ccc;
  background-color: #ffffff;
  padding: 1em;
}

.scroll-item {
  padding: 1em;
  border-bottom: 1px solid #eee;
}

.modal {
  position: fixed;
  inset: 0;
  background: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
}

.modal-content {
  background: white;
  padding: 1.5em;
  border-radius: 5px;
}

.audit-log {
  border: 1px solid #ccc;
  max-height: 400px;
  overflow: auto;
  background-color: rgb(240, 237, 237);
  padding-block: 2em;
}

.log-header {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 1em;
}

ul {
  list-style: none;
}

.audit-log li {
  font-size: 0.9rem;
  padding: 0.5em;
  border-bottom: 1px solid #eee;
}
</style>
