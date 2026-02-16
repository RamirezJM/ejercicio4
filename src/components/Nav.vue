<script setup>
</script>
<template>
  <nav>
    <h1>Centro de Eventos</h1>
    <ul>
      <li><a href="#">Nosotros</a></li>
      <li><a href="#">Eventos</a></li>
    </ul>
  </nav>
</template>

<style scoped>
nav {
  width: 100%;
  display: flex;
  align-items: center;
  padding: 1em 2em;
  background: linear-gradient(rgb(112, 106, 49), rgb(120, 126, 120));
}

ul{
  display: flex;
  list-style-type: none;
  gap: 2em;
}

h1 {
  margin-right: auto;
}

a {
  text-decoration: none;
  color: #323232;
  font-size: 1.2rem;
}
</style>