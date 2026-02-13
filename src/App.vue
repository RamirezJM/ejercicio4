<script setup>
import { ref } from 'vue'
import data from '../src/data/eventos.json'
import Tarjeta from './components/Tarjeta.vue';

const terminoBusqueda = ref('')
const mensajeBusqueda = ref('')
const eventLog = ref([])

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
  eventLog.value.unshift(texto)

  if (eventLog.value.length > 10) {
    eventLog.value.pop()
  }
}

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

</script>

<template>
  <h2>Cartelera de eventos</h2>
  <main>

    <section class="busqueda">
      <form @submit.prevent="buscar">

        <input v-model="terminoBusqueda" type="text" placeholder="Buscar evento..." @keyup.enter="buscar"
          @keydown.esc="limpiar" />

        <button type="submit">
          Buscar
        </button>

        <!-- Ejemplo inline para mezclar estilos -->
        <button type="button" @click="terminoBusqueda = ''">
          Vaciar (inline)
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

  </main>

</template>

<style>
.tarjetas {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: 1em;
  padding: 2em;
}
</style>
