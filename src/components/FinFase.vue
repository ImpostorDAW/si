<script setup>
defineProps({
  votado: Object,
  datosPartida: Object,
});

defineEmits(['reiniciar']);
</script>

<template>
  <div class="seccion">
    <div class="panelResultado resultadoExito">
      <h2>Acertaste!</h2>
      <p>
        <strong>{{ votado?.nombre }}</strong> era el impostor
      </p>
    </div>
    <div class="panelInfo">
      <p>
        <strong>Palabra:</strong>
        <span class="palabraGrande">{{ datosPartida.palabra?.palabra }}</span>
      </p>
      <p><strong>Categoria:</strong> {{ datosPartida.palabra?.categoria }}</p>
      <p><strong>Pista:</strong> {{ datosPartida.palabra?.pista }}</p>
    </div>
    <button class="boton botonEliminar botonAncho" @click="$emit('reiniciar')">
      Nueva partida
    </button>
  </div>
</template>
