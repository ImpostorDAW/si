<script>
export default {
  props: ['jugadores', 'impostorIndex', 'palabra'],
  methods: {
    reiniciar() {
      this.$emit('reiniciar');
    },
  },
};
</script>

<template>
  <div class="seccion">
    <div class="panelResultado resultadoFracaso">
      <h2>¡El impostor ha ganado!</h2>
      <p>
        <strong>{{ jugadores[impostorIndex] }}</strong> era el impostor y ha
        conseguido engañar a todos.
      </p>
      <div class="impostor-ganador">
        <p>
          El impostor ganador es:
          <span class="nombre-impostor">{{ jugadores[impostorIndex] }}</span>
        </p>
      </div>
    </div>
    <div class="panelInfo">
      <p>
        <strong>Palabra:</strong>
        <span class="palabraGrande">{{ palabra?.palabra }}</span>
      </p>
      <p><strong>Categoría:</strong> {{ palabra?.categoria }}</p>
      <p><strong>Pista:</strong> {{ palabra?.pista }}</p>
    </div>
    <button class="boton botonEliminar botonAncho" @click="reiniciar()">
      Nueva partida
    </button>
  </div>
</template>
