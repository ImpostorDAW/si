<script>
export default {
  props: ['jugadores', 'votadoIndex'],
  methods: {
    volverVotar() {
      this.$emit('volverVotar');
    },
  },
};
</script>

<template>
  <div class="seccion">
    <div class="panelResultado resultadoFallo">
      <h2>No es {{ jugadores[votadoIndex] }}</h2>
      <p>Sigue intentando...</p>
    </div>
    <button class="boton botonJugador botonAncho" @click="$emit('volverVotar')">
      Volver a votar
    </button>
  </div>
</template>
