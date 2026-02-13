<script setup>

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
      <button
    class="favorito"
    @click.stop="toggleFavorito"
  >
    {{ evento.favorito ? '❤' : '🤍' }}
  </button>
    <p>{{ evento.fecha }}</p>
  <p>{{ evento.categoria }}</p>
  <p>{{ evento.lugar }}</p>
  <p>{{ evento.descripcion }}</p>
  <img :src="evento.imagen" :alt="evento.titulo">
  </article>
 
</template>

<style>

article{
  display: flex;
  flex-direction: column;
  border: solid 2px #424242;
  border-radius: 5px;
  padding: 1em;

}
.activa {
  border: 2px solid #42b883;
  transform: scale(1.01);
}

img{
  order: -1;
}

</style>
