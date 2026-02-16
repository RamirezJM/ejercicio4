# ejercicio4
ejercicio 4 / Modulo VI - Vue

ejercicio desplegado: https://ramirezjm.github.io/ejercicio4/

[![MIT License](https://img.shields.io/badge/License-MIT-green.svg)](https://choosealicense.com/licenses/mit/)
![Static Badge](https://img.shields.io/badge/HTML5-%23f06529)
![Static Badge](https://img.shields.io/badge/CSS3-%232965f1)
![Static Badge](https://img.shields.io/badge/Javascript-%23f0db4f)
![Static Badge](https://img.shields.io/badge/Vue-2342b883)

App 'Centro de Eventos' que permite mostrar eventos del DOM en Vue, manejadores en línea y métodos, modificadores de eventos y teclas. Contiene un registro de eventos que permite ver los últimos 15 eventos ocurridos en el DOM.

## Búsqueda con enter / esc
'Enter' permite ejecutar búsqueda y 'esc' limpiar el cuadro de búsqueda, mostrando todos los resultados.

   ```HTML
   <input v-model="terminoBusqueda" type="text" placeholder="Buscar evento..." @keyup.enter="buscar"
          @keydown.esc="limpiar" />
   ```

<div>
  <img src="public/images/busqueda-enter.jpg" width=500>
  <img src="public/images/busqueda-enter-2.jpg" width=500>
</div>
<div>
  <img src="public/images/busqueda-esc.jpg" width=500>
  <img src="public/images/busqueda-esc-2.jpg" width=500>
</div>

## Click con stop
Un botón 'favorito' con @click.stop evita que el click se propague a la tarjeta

```HTML
<button class="favorito" @click.stop="toggleFavorito">
```

<div>
  <img src="public/images/click-favorito.jpg" width=500>
  <img src="public/images/click-favorito-2.jpg" width=500>
</div>

## Captura
El contenedor de tarjetas contiene @click.capture="logCaptura" que permite evidenciar el orden de captura vs propagación

```HTML
<section @click.capture="logCaptura" class="tarjetas">
```

<div>
  <img src="public/images/click-tarjeta.jpg" width=500>
  <img src="public/images/click-tarjeta-2.jpg" width=500>
</div>

## Acción única
Un botón 'Mostrar tips' funciona solo una vez, luego se deshabilita y cambia su texto.

```HTML
 <button @click.once="mostrarTips" :disabled="tipsMostrados">
    {{ tipsMostrados ? 'Tips ya mostrados' : 'Mostrar tips' }}
 </button>
```

<div><img src="public/images/accion-unica.jpg" width=500></div>
<div><img src="public/images/accion-unica-2.jpg" width=500></div>
<div><img src="public/images/accion-unica-3.jpg" width=500></div>

## Scroll pasive
Un contenedor con altura fija y varios elementos para forzar el scroll, reflejando la posición en un indicador.

```HTML
<div class="scroll-box" @scroll.passive="onScroll">
```

<div>
  <img src="public/images/scroll-pasive.jpg" width=500>
</div>

## Enlace prevent
Enlace que no navega, sino que abre un modal

```HTML
<a href="https://example.com" @click.prevent="abrirModalInfo">
   Más información del centro
</a>
```

<div><img src="public/images/enlace-prevent.jpg" width=500></div>
<div><img src="public/images/enlace-prevent-2.jpg" width=500></div>
<div><img src="public/images/enlace-prevent-3.jpg" width=500></div>


### Clonar el repositorio

  ```bash
   git clone https://github.com/RamirezJM/ejercicio4.git
   cd ejercicio4
  ```

### Instalar dependencias

```bash
npm install
```

### Levantar el servidor

```bash
npm run dev
```
