<script setup>
defineProps({
  votado: Object,
});

defineEmits(['volverVotar']);
</script>

<template>
  <div class="seccion">
    <div class="panelResultado resultadoFallo">
      <h2>No es {{ votado?.nombre }}</h2>
      <p>Sigue intentando...</p>
    </div>
    <button class="boton botonJugador botonAncho" @click="$emit('volverVotar')">
      Volver a votar
    </button>
  </div>
</template>
