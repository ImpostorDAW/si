<script>
export default {
  props: ['jugadores', 'votadoIndex', 'palabra'],
  methods: {
    reiniciar() {
      this.$emit('reiniciar');
    },
  },
};
</script>

<template>
  <div class="seccion">
    <div class="panelResultado resultadoExito">
      <h2>Acertaste!</h2>
      <p>
        <strong>{{ jugadores[votadoIndex] }}</strong> era el impostor
      </p>
    </div>
    <div class="panelInfo">
      <p>
        <strong>Palabra:</strong>
        <span class="palabraGrande">{{ palabra?.palabra }}</span>
      </p>
      <p><strong>Categoria:</strong> {{ palabra?.categoria }}</p>
      <p><strong>Pista:</strong> {{ palabra?.pista }}</p>
    </div>
    <button class="boton botonEliminar botonAncho" @click="$emit('reiniciar')">
      Nueva partida
    </button>
  </div>
</template>
