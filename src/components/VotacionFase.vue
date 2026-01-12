<script>
export default {
  props: ['jugadores', 'impostorIndex'],
  methods: {
    votar(i) {
      this.$emit('votar', i);
    },
  },
};
</script>

<template>
  <div class="seccion">
    <h2>Quien es el impostor?</h2>
    <div class="listaVotacion">
      <button
        v-for="(j, i) in jugadores"
        :key="j"
        class="boton botonJugador botonGrande"
        @click="votar(i)"
      >
        {{ j }}
      </button>
    </div>
  </div>
</template>
