<template>
  <img alt="Vue logo" src="./assets/logo.png">
  <!-- <HelloWorld msg="Welcome to Your Vue.js App"/> -->
  <Entrada tituloBonito="Prueba con axios"></Entrada>

  <hr>

  <UsuarioProyecto/>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'
import Entrada from './components/Entrada.vue';
import UsuarioProyecto from './components/UsuarioProyecto.vue';

export default {
  name: 'App',
  components: {
    HelloWorld,
    Entrada,
    UsuarioProyecto
  }
}
</script>

<style  >
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

textarea {
  resize : none
}
</style>
