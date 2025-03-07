<template>
  <div id="app">
    <NavBar />
    <main>
      <router-view />
    </main>
  </div>
</template>

<script>
import NavBar from './components/NavBar.vue';

export default {
  components: {
    NavBar
  }
};
</script>

<style>
/* Estilos globales */
#app {
  font-family: 'Arial', sans-serif;
  text-align: center;
}

main {
  padding: 20px;
}
</style>
