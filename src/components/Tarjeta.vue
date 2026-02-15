<script setup>

import red from '../assets/heart-red.png'
import white from '../assets/heart-white.png'

const { evento, seleccionada } = defineProps({
  evento: {
    type: Object,
    required: true
  },
  seleccionada: {
    type: Boolean,
    required: true
  }
})

const emit = defineEmits(['seleccionar', 'favorito'])

function seleccionar() {
  console.log('seleccionar disparado')
  emit('seleccionar', evento.id)
}

function toggleFavorito(e) {
  console.log('favorito disparado')
  emit('favorito', evento.id)
}

</script>

<template>

  <article @click="seleccionar" :class="{ activa: seleccionada }">
    <h3>{{ evento.titulo }}</h3>
    <p><strong>Fecha:</strong> {{ evento.fecha }}</p>
    <p><strong>Categoría:</strong> {{ evento.categoria }}</p>
    <p><strong>Lugar:</strong> {{ evento.lugar }}</p>
    <p>{{ evento.descripcion }}</p>
    <img :src="evento.imagen" :alt="evento.titulo">
    <button class="favorito" @click.stop="toggleFavorito">
      <!-- {{ evento.favorito ? red : white }} -->
      <img :src="evento.favorito ? red : white" alt="favorito" class="icono-favorito" />
      <small>Me gusta</small></button>
  </article>

</template>

<style>
article {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  border: solid 2px #424242;
  border-radius: 5px;
  padding: 1em;
  max-width: 400px;


}

.activa {
  border: 2px solid #42b883;
  transform: scale(1.01);
}

img {
  order: -1;
}

.favorito {
  border: none;
  width: fit-content;
  background: transparent;
}

.icono-favorito {
  width: 30px;
}
</style>
